# Artifact ownership (`JR-ORCH-003`)

Canonical contracts for **who writes which file** under `.cursor/orchestrations/{task-id}/`, required **section order**, **freeze** rules after handoff, and **per-file loop edit** behavior (append vs replace). **Routing**, counters, and `gate_status` on loops: [`loops-and-rework.md`](./loops-and-rework.md) (`JR-ORCH-006`). Role contracts in [`.cursor/agents/`](../templates/universal-agents/) implement these rules; this document is the platform source of truth targets copy into orchestration guides and topic rules.

**Prerequisites:** [`task-folder-model.md`](./task-folder-model.md) (`JR-ORCH-001`), [`task-manifest.md`](./task-manifest.md) (`JR-ORCH-002`).  
**Templates:** [`../templates/orchestration/_template/`](../templates/orchestration/_template/).

## Design goals

| Goal | How this document helps |
| --- | --- |
| **Agent efficiency** | One table + per-file section contracts — no guessing headings or edit rights |
| **Fresh-session resume** | Frozen Planner outputs and append-only build history preserve truth across loops |
| **Auditability** | Validator and human Gate 6 read stable filenames with predictable sections |
| **Separation** | Manifest routing vs stage facts — only Orchestrator writes JSON routing |

## Master ownership matrix

| File | Primary writer | Read by | Lifecycle gate(s) |
| --- | --- | --- | --- |
| `task-manifest.json` | **Orchestrator** | All roles | 0, 6 (+ routing for 1–5) |
| `plan.md` | **Planner** | Builder, Tester, Validator, Orchestrator | 1, 2 |
| `acceptance-criteria.md` | **Planner** | Builder (traceability), Tester, Validator | 1, 2 |
| `test-matrix.md` | **Planner** (Planned) → **Tester** (Actual) | Tester, Validator | 2 (plan), 4 (tests) |
| `build-log.md` | **Builder** | Tester, Validator, Orchestrator | 3 |
| `test-report.md` | **Tester** | Validator, Orchestrator | 4 |
| `validation-report.md` | **Validator** | Orchestrator, human (Gate 6) | 5 |
| `human-approval.md` | **Orchestrator** (from human decision) | Human, Orchestrator | 6 |

**Merge-ready and handoff vocabulary:** [`gates-and-checks.md`](./gates-and-checks.md) (`JR-ORCH-004`). Small-run skip path matrices — [`init-paths.md`](./init-paths.md) (`JR-ORCH-005`).

## Global rules

1. **Manifest:** Only the Orchestrator creates or updates `task-manifest.json`. Other roles are read-only; they do not patch `gate_status`, `current_agent`, or approval fields.
2. **Application source:** Lives in the normal repo tree per `plan.md` **Component/file map**. Stage markdown files record process and evidence — they are not product code.
3. **`locked_artifacts`:** Listed paths are read-only for all pipeline roles until the Orchestrator or human removes them from the manifest.
4. **Scope:** Do not expand requirements via orchestration edits. Design truth is `plan.md`; implementation truth is code + `build-log.md`; test truth is `test-report.md` (+ matrix when used).
5. **Merge-ready claims:** Only the Validator records merge-ready evidence in `validation-report.md` (**Checklist audit**, verdict). Builder and Tester do not claim merge-ready. Orchestrator sets `awaiting_human` only after the validation path documented in the target orchestration guide (normally Validator **PASS** / **PASS_WITH_NOTES**).
6. **No Jarvis/WFD IDs in target artifacts:** Replace placeholders with target `task_id` and project docs — no `JR-*`, `wfd-*`, or links to the Jarvis repository.
7. **Comments in templates:** HTML comments in `_template/` files are authoring hints. Delete them (or replace with real content) before stage handoff.

## Freeze and rework

**Manifest routing and counters** on validation remediation or human Gate 6 rework: [`loops-and-rework.md`](./loops-and-rework.md).

After the Orchestrator marks a gate **passed**, the owning role must not rewrite that stage’s artifact except as below.

| Artifact | Frozen after | Allowed later changes |
| --- | --- | --- |
| `plan.md`, `acceptance-criteria.md`, Planner `test-matrix.md` sections | `gate_2_plan` → `passed` | **Rework to Planner** (`rejected_rework` → `planner`, or scope-changing replan): Planner may replace files; append `rework_history` in manifest |
| `build-log.md` | — (never frozen) | **Append** dated sections on remediation/rework; do not delete prior loop evidence |
| `test-report.md`, Tester `test-matrix.md` sections | `gate_4_tests` → `passed` | **Replaced** on each Tester invocation (remediation loop re-runs Tester) |
| `validation-report.md` | — | **Replaced** on each Validator invocation (one current verdict per pass) |
| `human-approval.md` | `gate_6_human_approval` → `passed` and `status: complete` | Append-only **audit note** if human revokes post-close (rare); otherwise new run |

**Orchestrator** may add routing summaries to `completed_stages` and `command_evidence` — not full copies of stage files.

## Per-artifact contracts

### `plan.md` (Planner)

**Purpose:** Executable design — scope, file map, ADR implications, verified validation commands, risks.

**Required sections (fixed order):**

1. Objective restatement  
2. Risk and phase strategy  
3. Scope boundary (in-scope / out-of-scope)  
4. Phases (table + per-phase deliverables, files, dependencies)  
5. Component/file map  
6. Interface contracts  
7. ADR references (implications spelled out — Accepted ADRs only)  
8. Validation commands (verified project commands only; map to AC IDs where helpful)  
9. Risks  
10. Rollback  
11. Open questions  

**Template:** [`../templates/orchestration/_template/plan.md`](../templates/orchestration/_template/plan.md).

**Handoff:** Orchestrator must not invoke Builder until sections 1–11 exist and open questions do not block execution (or run is `blocked`).

---

### `acceptance-criteria.md` (Planner)

**Purpose:** Independently verifiable criteria for Tester and Validator.

**Format:**

- Title: `# Acceptance Criteria — {task_id}`  
- Grouped sections (e.g. Functional, Architectural, Test evidence) — target may add domain groups  
- Each criterion: `- [ ] AC-NN: …` with **zero-padded** IDs (`AC-01`, `AC-02`, …)

**Rules:**

- Every AC must be verifiable without unstated intent.  
- Include out-of-scope ACs when they prevent scope creep.  
- Do not duplicate TypeScript types in prose — describe behavior and boundaries.

**Template:** [`../templates/orchestration/_template/acceptance-criteria.md`](../templates/orchestration/_template/acceptance-criteria.md).

---

### `test-matrix.md` (Planner + Tester)

**Purpose:** Layer-level planned vs actual coverage; AC-level detail stays in `test-report.md`.

| Section | Owner | When |
| --- | --- | --- |
| **Planned coverage** (+ optional AC → layers summary) | Planner | Before Builder when required |
| **Actual coverage**, **Gaps vs plan**, **Residual risk** | Tester | Before Validator when required |

**When required:** `risk_tier.level` is `medium` or `large`, or any testable code change. Optional for `small` doc-only runs — map layers in `test-report.md` only.

**Layer IDs:** Use the target’s `docs/stack/testing-strategy.md` (or equivalent). Generic template uses `UNIT`, `COMP`, `INTG`, `E2E`, `MANUAL` — rename to match the project.

**Template:** [`../templates/orchestration/_template/test-matrix.md`](../templates/orchestration/_template/test-matrix.md).

**Handoff:** Planner must not edit **Actual coverage**. Tester must not edit **Planned coverage** except to fix Planner typos with Orchestrator approval (prefer note in `test-report.md` **Blockers**).

---

### `build-log.md` (Builder)

**Purpose:** Implementation record — files touched, command evidence, deviations, blockers.

**Required sections:**

1. Files created  
2. Files modified  
3. Command evidence  
4. Deviations from plan  
5. Scope pressure  
6. Unresolved open questions  
7. Known gaps  

**Loop behavior:** On remediation or `rejected_rework` to Builder, **append** a dated subsection (e.g. `## Remediation — loop 2 — 2026-05-19`) with new tables; preserve prior sections.

**Template:** [`../templates/orchestration/_template/build-log.md`](../templates/orchestration/_template/build-log.md).

**Handoff:** Orchestrator must not invoke Tester until `build-log.md` exists and reflects the current code state (or run is blocked).

---

### `test-report.md` (Tester)

**Purpose:** AC → test mapping, commands, gaps, blockers.

**Required sections:**

1. Coverage map (every AC ID)  
2. Uncovered criteria  
3. Test stability notes  
4. Command evidence  
5. Commands to run (copy-paste from verified project docs)  
6. Blockers  

**Loop behavior:** **Replace** the file on each Tester invocation so Validator always reads one current report.

**Template:** [`../templates/orchestration/_template/test-report.md`](../templates/orchestration/_template/test-report.md).

**Writes elsewhere:** Test **files** in the repo per plan and project conventions — not orchestration markdown except `test-matrix.md` when required.

---

### `validation-report.md` (Validator)

**Purpose:** Independent audit — verdict, AC/ADR/checklist evidence, remediations.

**Required sections:**

1. Verdict — exactly one of: `PASS`, `PASS_WITH_NOTES`, `FAIL`  
2. AC audit (✅ / ⚠️ / ❌ per AC ID)  
3. ADR compliance (per ADR cited in `plan.md`)  
4. Checklist audit (target `docs/validation-checklist.md` — **MG-*** and applicable domain rows)  
5. Test and command evidence  
6. Regressions  
7. Required remediations (numbered; mandatory when **FAIL**)  
8. Recommended remediations  

**Loop behavior:** **Replace** on each Validator pass. Orchestrator increments `loop_count` only on **FAIL** routed to Builder.

**Template:** [`../templates/orchestration/_template/validation-report.md`](../templates/orchestration/_template/validation-report.md).

---

### `human-approval.md` (Orchestrator)

**Purpose:** Durable Gate 6 record for humans and auditors; mirrors manifest `human_approval`.

**Required sections:**

1. **Evidence summary** — task id, objective, risk tier, files changed, AC coverage summary, validation verdict, commands run/not run, rework count, known follow-ups  
2. **Approval decision** — approver, outcome, ISO-8601 timestamp, conditions, notes  
3. **Rework directive** — fill only when outcome is `rejected_rework` (reason, return stage Builder or Planner)  
4. **Confirmation** — human acknowledgment line  

**Writer:** Orchestrator records facts the human states in chat. Humans do not edit this file ad hoc without Orchestrator sync — avoids drift from `task-manifest.json`.

**Mirror rule:** When Gate 6 is recorded, `human_approval` in the manifest and `human-approval.md` **must match** (`status`, `approved_at`, `approver`, `conditions`, `notes`).

**Template:** [`../templates/orchestration/_template/human-approval.md`](../templates/orchestration/_template/human-approval.md).

**After approval:** Orchestrator may set `status: complete`. Ask whether to `git commit` — do not commit without explicit human confirmation in the same thread.

## Bootstrap sequence

```text
1. Copy _template/* → .cursor/orchestrations/{task-id}/
2. Rename task-manifest.example.json → task-manifest.json; set task_id, objective, risk_tier
3. Orchestrator → Planner (when in pipeline): plan.md, acceptance-criteria.md, [test-matrix.md]
4. Orchestrator → Builder: implementation + build-log.md
5. Orchestrator → Tester: tests + test-report.md [, test-matrix actual]
6. Orchestrator → Validator: validation-report.md
7. Orchestrator → human: human-approval.md + manifest human_approval
```

Skipped stages must align with `risk_tier.skipped_stages` and `gate_status` ([`init-paths.md`](./init-paths.md) for tier matrices).

## Cross-artifact dependencies

| Consumer | Must read before acting |
| --- | --- |
| Builder | `plan.md`, `acceptance-criteria.md`, manifest (`locked_artifacts`) |
| Tester | `acceptance-criteria.md`, `build-log.md`, plan (context only) |
| Validator | `plan.md`, `acceptance-criteria.md`, `build-log.md`, `test-report.md`, checklist doc |
| Orchestrator (Gate 6) | `build-log.md`, `test-report.md`, `validation-report.md` (when Validator ran), manifest |

## Target adoption

| Step | Action |
| --- | --- |
| 1 | Copy Jarvis [`../templates/orchestration/_template/`](../templates/orchestration/_template/) into target `.cursor/orchestrations/_template/` |
| 2 | Sanitize and wire per [`self-containment.md`](./self-containment.md) (`JR-ORCH-007`) — placeholders, strip template callouts, `_template/README.md` |
| 3 | Add `PROJ-ORCH-*` row(s) in target `docs/roadmap/backlog.md` if not already present |
| 4 | Point target `.cursor/agents/*.md` and orchestration guide at this contract (paraphrase — no Jarvis links in target artifacts) |
| 5 | Run **ORCH-IND-01–10** in target `docs/handoff-self-containment.md` before marking `PROJ-ORCH-001` / handoff rows complete |
| 6 | Add topic rule `orchestration-artifacts` (optional) summarizing the master matrix |

## Decisions recorded for `JR-ORCH-003`

Defaults favor long-term agent clarity; override in the target orchestration guide when the user directs:

| Topic | Decision |
| --- | --- |
| Planner freeze | After `gate_2_plan` is `passed`, Planner does not edit `plan.md` / ACs / planned matrix except via rework to Planner |
| Build log on loops | **Append** dated sections — preserve audit trail |
| Test / validation reports on loops | **Replace** each invocation — single current truth |
| `test-matrix.md` | Dual sections; Planner vs Tester ownership split |
| Human approval file | **Orchestrator** writes; must mirror manifest |
| AC ID format | `AC-NN` zero-padded |
| Template pack | All seven markdown artifacts + manifest example ship in Jarvis `_template/` (`JR-ORCH-003`) |

## Related

| Doc | Role |
| --- | --- |
| [`task-folder-model.md`](./task-folder-model.md) | Paths, git policy, gate vocabulary pointer |
| [`task-manifest.md`](./task-manifest.md) | JSON fields |
| [`README.md`](./README.md) | Orchestration pack index |
| [`../templates/universal-agents/`](../templates/universal-agents/) | Per-role rules and output contracts |
| [`gates-and-checks.md`](./gates-and-checks.md) | Lifecycle vs merge-ready vs handoff |
| [`loops-and-rework.md`](./loops-and-rework.md) | Remediation vs human rework routing (`JR-ORCH-006`) |
| [`self-containment.md`](./self-containment.md) | Copy sanitize and **ORCH-IND-*** (`JR-ORCH-007`) |
| [`../universal-validation/README.md`](../universal-validation/README.md) | Checklist and MG-* |
