# Lifecycle gates, merge-ready checks, and handoff (`JR-ORCH-004`)

Canonical separation of **three gate vocabularies** used in orchestrated development and target initialization. Agents must not conflate pipeline stage completion, merge-ready claims, or Jarvis independence verification.

**Prerequisites:** [`task-folder-model.md`](./task-folder-model.md), [`task-manifest.md`](./task-manifest.md), [`artifact-ownership.md`](./artifact-ownership.md)  
**Checklist IDs:** [`../templates/universal-validation/validation-checklist.md`](../templates/universal-validation/validation-checklist.md) (**MG-***, **TST-***)  
**Handoff layer:** [`../universal-handoff/README.md`](../universal-handoff/README.md) — not a fourth lifecycle gate  
**Small-run path matrices:** [`init-paths.md`](./init-paths.md) (`JR-ORCH-005`)

## Design goals

| Goal | How this document helps |
| --- | --- |
| **Agent efficiency** | One place for vocabulary, mapping, and who may claim what — avoids re-deriving from WFD or chat |
| **Fresh-session resume** | `gate_status` vs checklist IDs vs `PROJ-HANDOFF-*` are unambiguous in manifest and artifacts |
| **Readable audits** | Humans and Validator use the same MG-* IDs in PR, checklist, and `validation-report.md` |
| **Target ownership** | Targets paraphrase into orchestration guide + `workflow-gates.mdc` — no Jarvis links in target artifacts |

## Three vocabularies (do not conflate)

| Vocabulary | Range / IDs | Question it answers | Primary record |
| --- | --- | --- | --- |
| **Lifecycle gates** | **0–6** | Did this **pipeline stage** complete for this run? | `task-manifest.json` → `gate_status` |
| **Merge-ready checks** | **MG-01**–**MG-05** | May this change be claimed **merge-ready** (ADR, gaps, validation, tests, tooling)? | `docs/validation-checklist.md`, `validation-report.md`, PR body |
| **Handoff checks** | **PROJ-HANDOFF-*** (target backlog) | Is the **initialized project** independent of Jarvis (no runtime/doc dependency on the scaffolding repo)? | `docs/handoff-self-containment.md`, target `docs/roadmap/backlog.md` |

**Common mistakes:**

| Mistake | Correct mental model |
| --- | --- |
| Lifecycle **Gate 5** passed ⇒ merge-ready | Gate 5 = Validator stage green; **MG-01**–**MG-05** must still be satisfied (Gate 5 audits MG-* at validation) |
| Lifecycle **Gate 6** passed ⇒ merge-ready | Gate 6 = human approval **after** merge-ready path is green; Gate 6 does not replace **MG-*** |
| `status: complete` ⇒ safe to merge PR | `complete` closes the **orchestration run**; merge still needs branch/CI/review per team process |
| `PROJ-HANDOFF-001` done ⇒ current task merge-ready | Handoff verifies **initialization** independence, not a feature run |
| `gate_3_build: passed` ⇒ **MG-05** green | Build complete ≠ tooling pass; **MG-05** may fail at Gate 5 even when Gate 3 passed |

**Naming:** Use **MG-01**–**MG-05** in PRs and agent text. Retire legacy “merge-ready Gate 1–5” numbering (it did not match MG order).

## Lifecycle gates (0–6)

Lifecycle gates track **orchestration pipeline stage completion** for a single run under `.cursor/orchestrations/{task-id}/`.

| Gate | `gate_status` key | Primary owner | Stage artifact(s) |
| --- | --- | --- | --- |
| **0** Intake and risk tier | `gate_0_intake` | Orchestrator | Manifest `risk_tier`, `pipeline`, skips |
| **1** Requirements freeze | `gate_1_requirements` | Planner | `acceptance-criteria.md` |
| **2** Executable plan | `gate_2_plan` | Planner | `plan.md` [, planned `test-matrix.md`] |
| **3** Build complete | `gate_3_build` | Builder | `build-log.md` + implementation |
| **4** Tests mapped | `gate_4_tests` | Tester | `test-report.md` [, actual `test-matrix.md`] |
| **5** Validation green | `gate_5_validation` | Validator | `validation-report.md` |
| **6** Human approval | `gate_6_human_approval` | Orchestrator | `human-approval.md` + manifest `human_approval` |

**Per-key values:** `pending`, `passed`, `failed`, `blocked`, `skipped`, `n/a` — see [`task-manifest.md`](./task-manifest.md#gate-status).

**Gate 0** sets `risk_tier.level` and documents skipped pipeline roles in `risk_tier.skipped_stages`. It does not satisfy any **MG-*** row by itself.

**Gate 6** is always required for **code-changing** orchestrated runs (not skipped via `risk_tier`). It records human sign-off; it is **not** a checklist ID.

### Manifest `status` vs lifecycle gates

| `status` | Meaning | Typical gate / MG context |
| --- | --- | --- |
| `in_progress` | Pipeline active | One or more lifecycle gates not `passed` / appropriately `skipped` |
| `awaiting_human` | Validation path green (or documented small-run substitute) | **MG-01**–**MG-05** satisfied per policy; **Gate 6** pending |
| `complete` | Human approved; run closed | **Gate 6** `passed`; `human_approval` recorded |
| `blocked` | Cannot proceed | Failed gate, max loops, or unresolved contradiction |

Orchestrator sets `awaiting_human` only after the merge-ready path documented in the target orchestration guide (normally Validator **PASS** / **PASS_WITH_NOTES** with full **MG-*** evidence). Orchestrator sets `complete` only after **Gate 6** is recorded in manifest and `human-approval.md`.

## Merge-ready checks (MG-01–MG-05)

Merge-ready checks define whether agents or humans may claim work is **merge-ready** (ready for merge per project policy — ADR compliance, evidence artifacts, tests, tooling).

| ID | Topic | Typical lifecycle stage | Primary evidence |
| --- | --- | --- | --- |
| **MG-01** | Alignment gaps | 5 (audit) | `validation-report.md` → ADR compliance; target alignment-gaps doc |
| **MG-02** | ADR before durable architecture | 2 (plan), 5 (re-check) | ADR file(s) + `plan.md` → ADR references |
| **MG-03** | Validation evidence + domain rows | 5 | `validation-report.md` (verdict + checklist audit) |
| **MG-04** | Test evidence (**TST-05**) | 4 (primary), 5 (cross-check) | `test-report.md`; `test-matrix.md` when required |
| **MG-05** | Tooling (check, lint, …) | 3–5 | `build-log.md`, `test-report.md`, validation command section |

**Domain checklist rows** (**SEC-**, **AUTH-**, stack extensions, ADR annexes) are audited at **lifecycle Gate 5** when applicable — they are **not** separate lifecycle gates. **TST-*** rows are owned at **Gate 4**; Validator cross-checks at Gate 5.

Row wording lives in the target [`validation-checklist.md`](../templates/universal-validation/validation-checklist.md) template. Agent-facing merge-ready policy should also live in target `.cursor/rules/workflow-gates.mdc` (from [`../templates/universal-rules/workflow-gates.mdc`](../templates/universal-rules/workflow-gates.mdc)).

### Who may claim merge-ready

| Role | May claim merge-ready? |
| --- | --- |
| **Validator** | Records verdict and **Checklist audit** in `validation-report.md` — does not replace Gate 6 |
| **Orchestrator** | May set `awaiting_human` after merge-ready path is green — not `complete` until Gate 6 |
| **Builder, Tester, Planner** | **No** — must not say “done”, “ready to merge”, or “merge-ready” for the task |
| **Human (PR)** | May state merge-ready in PR when **MG-*** satisfied per guide; Gate 6 still required for orchestrated `complete` |

## Lifecycle ↔ merge-ready mapping

Use this table when updating `gate_status` or writing `validation-report.md` → **Checklist audit**.

| Lifecycle gate | Merge-ready / checklist touchpoints |
| --- | --- |
| **0** | None (risk tier and skips only) |
| **1** | Feeds **TST-01** via AC IDs |
| **2** | **MG-01** / **MG-02** evidence prepared in `plan.md` |
| **3** | Command evidence supports **MG-05** |
| **4** | **MG-04**, **TST-01**–**TST-05** |
| **5** | **MG-03** + all applicable **MG-*** and domain rows |
| **6** | None — human approval after merge-ready path green |

**Lifecycle Gate 5** (validation green) **≠** **MG-05** (tooling). Validator owns Gate 5 and audits **MG-05** (and other MG rows) in the same pass.

## Orchestrated vs non-orchestrated work

| Work style | Lifecycle gates | Merge-ready |
| --- | --- | --- |
| **Orchestrated** (`.cursor/orchestrations/{task-id}/`) | Full **0–6** in `gate_status` | Full **MG-01**–**MG-05** when Validator runs; reduced set when stages skipped per policy (`JR-ORCH-005`) |
| **Non-orchestrated** (no task folder) | No `gate_status` | **MG-01**, **MG-02**, **MG-05** minimum for architecture-touching changes; **MG-04**-style test evidence in PR; **MG-03** only if `validation-report.md` exists |
| **Trivial** (typo, comment-only, non-architectural) | Usually N/A | **adr-compliance** only; note “orchestration not required” when relevant |

“Architecture-touching” matches high-impact areas in target **adr-compliance** (architecture, data flow, auth, sync, AI, offline/caching, secrets — project-defined).

## Small-run substitutes (summary)

When `risk_tier.level` is `small` and stages are skipped, record skips in `risk_tier.skipped_stages`, matching `gate_status` keys, and `flags` / rationale.

| Path | Flow (abbrev.) | Full orchestrated merge-ready? |
| --- | --- | --- |
| **A** | Builder → Tester (Validator skipped) | **No** — **MG-03** via human substitute audit + PR; Orchestrator may still set `awaiting_human` after Tester |
| **B** | Builder → Validator (Tester skipped, non-testable only) | **Yes** when **MG-01**–**MG-05** green via `validation-report.md` |
| **C** | Builder-only (both Tester and Validator skipped) | **No** for architecture-touching work without user approval |

**Gate 6** is not skipped for code-changing runs. Do not skip Validator on architecture-touching work without explicit human approval.

Full path tables and tier matrices: [`init-paths.md`](./init-paths.md).

## Handoff checks (separate from lifecycle and merge-ready)

**Handoff** answers: “Can this **target repository** operate without Jarvis?” It does **not** gate a single orchestration run.

| Layer | Artifact | Scope |
| --- | --- | --- |
| 1 | Target root `README.md` | Self-contained product description |
| 2 | `docs/handoff-self-containment.md` | No Jarvis links, broken refs, or chat-only requirements (`PROJ-HANDOFF-001`, `002`) |
| 3 | [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | Required `PROJ-*` rows + human sign-off (`PROJ-HANDOFF-003`) |

Jarvis may mark initialization handoff complete only when layer 2–3 criteria are met. That is independent of whether a feature run reached **Gate 6** or **MG-***.

## Document precedence (targets)

When documents conflict during an orchestrated run:

1. **Accepted** ADRs and `adrs/GOVERNANCE.md`
2. Target **workflow / merge-ready** rule (`workflow-gates.mdc`) and orchestration guide
3. Agent role contracts (`.cursor/agents/*.md`)
4. Orchestration templates and stage markdown in the task folder
5. This Jarvis platform doc (reference while initializing targets only — not linked from target artifacts)

## Target adoption

| Step | Action |
| --- | --- |
| 1 | Copy [`../templates/universal-rules/workflow-gates.mdc`](../templates/universal-rules/workflow-gates.mdc) → `.cursor/rules/workflow-gates.mdc`; replace placeholders |
| 2 | Ensure `docs/validation-checklist.md` orchestration appendix matches gate keys in [`task-manifest.md`](./task-manifest.md) |
| 3 | Add narrative tables to target orchestration guide (adapt sections from this doc — no Jarvis URLs) |
| 4 | Register `workflow-gates.mdc` in `.cursor/rules/index.md` (Workflow category, globs) |
| 5 | Optional: `validation-checklist.mdc` globs — see [`../universal-validation/README.md`](../universal-validation/README.md) |

## Human input (pause points)

Jarvis must **stop and ask** before:

- Renaming **MG-** or `gate_*` keys already referenced in Accepted ADRs, open `PROJ-*` tasks, or shipped templates.
- Skipping Validator on architecture-touching orchestrated work without explicit approval.
- Treating **handoff complete** as waiver for **MG-*** on a feature PR.
- Replacing the three-vocabulary model with a single “gates” checklist (hurts long-term agent parsing).

Routine copy, placeholder replacement, and aligning checklist appendix keys with manifest defaults do not require extra approval.

## Decisions recorded for `JR-ORCH-004`

| Topic | Decision |
| --- | --- |
| Canonical platform doc | This file — `gates-and-checks.md` |
| Vocabulary count | **Three:** lifecycle, merge-ready, handoff |
| Lifecycle range | **0–6** with fixed `gate_status` keys per [`task-manifest.md`](./task-manifest.md) |
| Merge-ready IDs | **MG-01**–**MG-05** in checklist; agent contract in `workflow-gates.mdc` template |
| Gate 5 vs MG-05 | Explicitly distinct; called out in all three surfaces |
| Gate 6 vs MG-* | Gate 6 follows green merge-ready path; does not substitute MG rows |
| `awaiting_human` vs `complete` | Merge-ready path → awaiting human → Gate 6 → `complete` |
| Handoff | **PROJ-HANDOFF-*** / self-containment — not lifecycle gates |
| Small-run detail | Summary only; [`init-paths.md`](./init-paths.md) owns tier matrices |
| WFD source | Dual vocabulary + mapping discipline — no WFD product/stack identifiers |

## Related

| Doc | Role |
| --- | --- |
| [`README.md`](./README.md) | Orchestration pack index |
| [`init-paths.md`](./init-paths.md) | Init and run-sizing paths (`JR-ORCH-005`) |
| [`task-folder-model.md`](./task-folder-model.md) | Paths, git policy |
| [`artifact-ownership.md`](./artifact-ownership.md) | Who writes merge-ready evidence |
| [`../universal-validation/README.md`](../universal-validation/README.md) | Checklist scaffold |
| [`../universal-handoff/README.md`](../universal-handoff/README.md) | Handoff scaffold |
| [`../roadmap/platform-spec.md`](../roadmap/platform-spec.md) | WFD concepts to generalize |
