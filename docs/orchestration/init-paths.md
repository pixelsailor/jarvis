# Initialization and run-sizing paths (`JR-ORCH-005`)

Canonical **small / medium / large** paths for (1) **target-project initialization** (Jarvis scaffolding) and (2) **orchestrated runs** (task-folder execution). Agents use this document for tier matrices, skip policy, and merge-ready substitutes — not chat memory.

**Prerequisites:** [`task-folder-model.md`](./task-folder-model.md), [`task-manifest.md`](./task-manifest.md), [`gates-and-checks.md`](./gates-and-checks.md) (`JR-ORCH-004`)  
**Target README routing:** [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) (init path summary)  
**Checklist IDs:** [`../templates/universal-validation/validation-checklist.md`](../templates/universal-validation/validation-checklist.md) (**MG-***, **TST-***)

## Two vocabularies (do not conflate)

| Term | Scope | Set when | Recorded in |
| --- | --- | --- | --- |
| **Initialization path** | Whole target scaffold (README → backlog → ADRs → rules → agents → orchestration) | Once per project at intake (Q7) | Target README, `docs/roadmap/backlog.md` (`PROJ-*`), session notes |
| **Run risk tier** | A single orchestrated objective under `.cursor/orchestrations/{task-id}/` | Gate 0 on each run | `task-manifest.json` → `risk_tier` |

A **small init** does not forbid **medium** or **large** runs later. A **large init** often uses **medium** or **large** run tiers for individual `PROJ-ORCH-*` tasks while scaffolding.

## Design goals

| Goal | How this document helps |
| --- | --- |
| **Agent efficiency** | One matrix per tier; manifest `pipeline` / `gate_status` / **MG-*** outcomes are lookup tables |
| **Fresh-session resume** | Tier and path letter (A/B/C) are explicit in manifest `flags` and rationale |
| **Readable audits** | Humans see why Validator was skipped and what substitutes **MG-03** |
| **Target ownership** | Targets paraphrase into orchestration guide — no Jarvis URLs in target artifacts |

---

## Part A — Project initialization paths

Choose **once** at intake ([`../target-readme/intake-questions.md`](../target-readme/intake-questions.md) Q7). When unclear, **default to medium** and tell the user.

### A.1 Path summary

| Init path | README & docs | Typical `PROJ-*` backlog | Orchestration during init |
| --- | --- | --- | --- |
| **Small** | Full required README outline; minimal `docs/` (governance + handoff + **validation checklist**) | `PROJ-README-*`, `PROJ-RULE-*` (1–2 rules), `PROJ-DOC-*` (incl. minimal `validation-checklist.md`), `PROJ-HANDOFF-*` | **No** task folders; linear Jarvis sessions |
| **Medium** | + boundaries refined; ADR index/governance; doc conventions; validation checklist; stack profile/docs | + `PROJ-ADR-*`, `PROJ-DOC-*`, `PROJ-STACK-*` | **Optional** task folder for unusually large doc/rule batches only |
| **Large** | + roadmap direction; full agent + orchestration scaffold | + `PROJ-AGENT-*`, `PROJ-ORCH-*`, validation + workflow rules | **Recommended** — use task folders for multi-step init work |

### A.2 Decision signals

| Signal | Favors |
| --- | --- |
| Greenfield, single developer, “README + a few rules only” | **Small** |
| Existing repo, needs ADRs + conventions + stack docs, no multi-agent pipeline yet | **Medium** (default) |
| Multi-session init, many `PROJ-*` areas, agent contracts, orchestration templates, evidence on backlog | **Large** |
| User explicitly wants agents + orchestration from day one | **Large** |
| User wants minimal scaffold; will add ADRs later | **Small** (still requires minimal `validation-checklist.md`; confirm handoff meets `PROJ-HANDOFF-*`) |
| Conflicting signals (e.g. “small” + full agent roster) | **Pause** — ask user (see [Human input](#human-input-pause-points)) |

### A.3 `PROJ-*` scope matrix (by init path)

| Area | Small | Medium | Large |
| --- | --- | --- | --- |
| `PROJ-README-*` | Required | Required | Required |
| `PROJ-RULE-*` | 1–2 always-apply rules | Index + always-apply + topic rules as needed | Full rules index per [`../universal-rules/README.md`](../universal-rules/README.md) |
| `PROJ-ADR-*` | Defer or single placeholder ADR | Index, governance, template; Accepted ADRs for README non-negotiables | Same + ADRs for each boundary that affects implementation |
| `PROJ-DOC-*` | `readme-governance`, `documentation-conventions`, **minimal `validation-checklist.md`** (required on all paths) | + `pr-and-commit-guide`, architecture/guides as README map requires | Full doc map from README |
| `PROJ-STACK-*` | Only if stack detected and README claims a stack | `stack-profile`, `commands`, `testing-strategy`, `runtime-boundaries` as applicable | + dependency review when warranted |
| `PROJ-AGENT-*` | **None** | Optional INDEX-only (roster without contracts) | **Required** INDEX + five role contracts ([`../universal-agents/README.md`](../universal-agents/README.md)) |
| `PROJ-ORCH-*` | **None** | Optional `_template/` copy only | `_template/` + orchestration guide + `workflow-gates.mdc` |
| `PROJ-HANDOFF-*` | Required | Required | Required |

### A.4 Scaffolding order (all paths)

Default sequence from [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md):

1. README (draft or audit-complete)  
2. `docs/roadmap/` (index + `PROJ-*` from this map)  
3. `adrs/` when path is medium or large (or README non-negotiables demand it)  
4. `.cursor/rules/`  
5. Topic docs (PR guide, validation checklist, stack docs) per path  
6. `.cursor/orchestrations/_template/` + agents + orchestration guide — **large only** (do not run step 6 before step 3 when README boundaries are still weak)  
7. `docs/handoff-self-containment.md` + `PROJ-HANDOFF-*` verification (include **ORCH-IND-*** when orchestration scaffolded — [`self-containment.md`](./self-containment.md))  

### A.5 Using orchestration during init (large / selective medium)

When init work itself needs resumability:

| Init task type | Suggested **run** tier | Notes |
| --- | --- | --- |
| Copy `_template/` and wire paths | **Small** path C or single Builder session | No Planner if objective is copy-only |
| Author orchestration guide + adapt gates doc | **Medium** | Full pipeline once |
| Multi-phase ADR + rules + agents + orch | **Large** | Planner phases; one task id per phase or one large objective with phased `plan.md` |

Use `{project-slug}-NNN` task ids (e.g. `acme-001`) documented in the target orchestration guide. Do not mix init run state into `docs/roadmap/backlog.md` — backlog tracks **setup tasks**; task folders track **execution** of a defined objective.

---

## Part B — Orchestrated run risk tiers

Set at **lifecycle Gate 0** before the first downstream agent. Record `risk_tier.level`, `rationale`, and any `skipped_stages` in `task-manifest.json`.

### B.1 Tier summary

| Run tier | Typical use | Minimum pipeline (stage agents) | Planner | Tester | Validator |
| --- | --- | --- | --- | --- | --- |
| **Small** | One or two files; low architectural risk; objective already actionable | Path **A**, **B**, or **C** below | Often skipped | Often runs (A) or skipped (B/C) | Often skipped (A/C) or runs (B) |
| **Medium** | Single feature area; several files; normal behavior change | `planner` → `builder` → `tester` → `validator` | Required | Required | Required |
| **Large** | Cross-cutting, ADR-governed, migrations, security boundaries | Planner + **phased** B→T→V loops | Required | Per phase | Per phase |

**Gate 6** is **never** skipped for code-changing runs. **Gate 6** does not satisfy **MG-*** by itself.

### B.2 Tier elevation (defaults to at least medium)

Orchestrator MUST classify as **`medium` or `large`** (not `small`) when the change touches any of the following — use project-defined lists in README § Boundaries and **adr-compliance** / high-impact areas:

| Category | Examples (generic) |
| --- | --- |
| **Durable architecture** | New boundaries, storage authority, public API contracts |
| **ADR-governed areas** | Any **Accepted** ADR cited or implicated by the objective |
| **Security-sensitive** | Auth, secrets, permissions, crypto, PII handling |
| **Data authority / sync** | System of record, conflict resolution, offline/caching policy (when the project defines them) |
| **Multi-file cross-cutting** | Touches more than one bounded context or layer |
| **Explicit user directive** | User requests full pipeline |

`large` additionally applies when `plan.md` needs **multiple Builder → Tester → Validator phases** or phased delivery with frozen AC subsets per phase.

### B.3 Medium run — full matrix

| Field | Value |
| --- | --- |
| `risk_tier.level` | `medium` |
| `pipeline` | `["planner", "builder", "tester", "validator"]` |
| `skipped_stages` | `[]` (empty) |
| `gate_status` | Gates 0–6: run each stage → `passed` (or `failed` / `blocked` on escalation); none `skipped` except `n/a` where inapplicable |
| **MG-01**–**MG-05** | Full set via `validation-report.md` on **PASS** / **PASS_WITH_NOTES** |
| `awaiting_human` | After Validator green merge-ready path |
| **Full orchestrated merge-ready?** | **Yes**, when **MG-*** green |

`test-matrix.md`: **required** when any testable code changes ([`artifact-ownership.md`](./artifact-ownership.md)).

### B.4 Large run — full matrix

| Field | Value |
| --- | --- |
| `risk_tier.level` | `large` |
| `pipeline` | `["planner", "builder", "tester", "validator"]` (same agents; **phased** execution) |
| `skipped_stages` | `[]` unless human-approved exception documented |
| Planner | `plan.md` lists phases; each phase has scope, AC subset, validation commands |
| Loops | [`loops-and-rework.md`](./loops-and-rework.md) — `loop_count` / `max_loops` **per phase** (default); reset at phase boundary |
| **MG-*** | Full audit at end of each Validator pass relevant to that phase; final Validator pass covers whole change set |
| **Full orchestrated merge-ready?** | **Yes**, when final Validator pass and **MG-*** green |

Do **not** skip Planner or Validator on `large` without explicit human approval and manifest `flags`.

### B.5 Small run — path A / B / C

Applies only when Gate 0 confirms `risk_tier.level: small` **and** [B.2 elevation](#b2-tier-elevation-defaults-to-at-least-medium) does not apply.

Record the chosen path in `risk_tier.rationale` and `flags` (e.g. `small_run_path_a`).

#### Path A — Builder + Tester (Validator skipped)

| Field | Value |
| --- | --- |
| **Flow** | Orchestrator → Builder → Tester → Orchestrator → Gate 6 |
| `pipeline` | `["builder", "tester"]` |
| `skipped_stages` | `["planner", "validator"]` (add `planner` only when objective is already actionable in manifest) |
| `gate_status` | `gate_1_requirements`, `gate_2_plan`: `skipped` or `n/a`; `gate_5_validation`: `skipped` after Tester |
| `flags` | `skipped_validator_small_run`, `substitute_audit_owner: human` (or named role) |
| **MG-01**, **MG-02** | PR + ADR evidence if architecture touched; prefer **medium** if ADR-governed |
| **MG-03** | **Substitute:** human PR review + checklist — **not** `validation-report.md` |
| **MG-04** | `test-report.md` |
| **MG-05** | `build-log.md` / `test-report.md` / verified commands |
| `awaiting_human` | After `test-report.md` complete |
| **Full orchestrated merge-ready?** | **No** |

#### Path B — Builder + Validator (Tester skipped, non-testable only)

| Field | Value |
| --- | --- |
| **Flow** | Orchestrator → Builder → Validator → Orchestrator → Gate 6 |
| `pipeline` | `["builder", "validator"]` |
| `skipped_stages` | `["planner", "tester"]` |
| `gate_status` | `gate_4_tests`: `skipped` with rationale (doc-only, copy, config with no test surface) |
| `flags` | `skipped_tester_non_testable` |
| **MG-04** | PR text or Validator notes — document **untested risk** |
| **MG-03** | `validation-report.md` **PASS** / **PASS_WITH_NOTES** |
| `awaiting_human` | After Validator green |
| **Full orchestrated merge-ready?** | **Yes**, when **MG-01**–**MG-05** green in report |

Use only when **no** automated test could reasonably run. Prefer path **A** when tests exist.

#### Path C — Builder only

| Field | Value |
| --- | --- |
| **Flow** | Orchestrator → Builder → Orchestrator → Gate 6 |
| `pipeline` | `["builder"]` |
| `skipped_stages` | `["planner", "tester", "validator"]` |
| `gate_status` | `gate_4_tests`, `gate_5_validation`: `skipped`; Gates 1–2 `skipped` when Planner skipped |
| `flags` | `small_run_path_c`, `substitute_audit_owner: human` |
| **MG-03**, **MG-04** | Human substitute audit + PR evidence |
| `awaiting_human` | After `build-log.md` complete |
| **Full orchestrated merge-ready?** | **No** |

Trivial non-code or single-file changes with **no** test surface and **no** architecture touch. Prefer path **A** when any test could run.

#### Small-run skip reference (by skipped stage)

| Skipped stage | `gate_status` keys | Checklist / merge-ready |
| --- | --- | --- |
| **Planner** | `gate_1_requirements`, `gate_2_plan` → `skipped` | **MG-01** / **MG-02** from PR/ADR if architecture touched — not `plan.md` |
| **Tester** | `gate_4_tests` → `skipped` | **MG-04** via PR (**MG-04** style); document untested risk |
| **Validator** | `gate_5_validation` → `skipped` | **MG-03** via human substitute — **no** full orchestrated merge-ready |

### B.6 Manifest examples (small paths)

**Path A** (minimal):

```json
"risk_tier": {
  "level": "small",
  "rationale": "Single-file UI copy fix; objective actionable; Path A",
  "skipped_stages": ["planner", "validator"]
},
"pipeline": ["builder", "tester"],
"flags": ["small_run_path_a", "skipped_validator_small_run", "substitute_audit_owner: human"]
```

**Path B**:

```json
"risk_tier": {
  "level": "small",
  "rationale": "Documentation-only; no test surface; Path B",
  "skipped_stages": ["planner", "tester"]
},
"pipeline": ["builder", "validator"],
"flags": ["small_run_path_b", "skipped_tester_non_testable"]
```

**Path C**:

```json
"risk_tier": {
  "level": "small",
  "rationale": "Comment-only; Path C",
  "skipped_stages": ["planner", "tester", "validator"]
},
"pipeline": ["builder"],
"flags": ["small_run_path_c", "substitute_audit_owner: human"]
```

### B.7 Non-orchestrated work (no task folder)

No `gate_status`. Use tier concepts informally:

| Scope | Merge-ready bar |
| --- | --- |
| Architecture-touching | **MG-01**, **MG-02**, **MG-05** + PR test evidence (**MG-04** style) |
| Trivial | **adr-compliance** only; note “orchestration not required” when relevant |

See [`gates-and-checks.md`](./gates-and-checks.md) § Orchestrated vs non-orchestrated.

---

## Part C — Cross-reference: init path ↔ run tier

| Init path | Typical first orchestrated run (if any) | Default run tier for feature work after init |
| --- | --- | --- |
| **Small** | None during init | **Small** or non-orchestrated for trivial fixes; **medium** for features |
| **Medium** | Optional medium run for big doc batches | **Medium** |
| **Large** | **Medium** or **large** for `PROJ-ORCH-*` / `PROJ-AGENT-*` | **Medium** default; **large** for cross-cutting |

---

## Target adoption

| Step | Action |
| --- | --- |
| 1 | Paraphrase Part A (init) and Part B (run tiers + paths A/B/C) into target orchestration guide — no Jarvis URLs |
| 2 | Add project-specific elevation rows to B.2 (boundaries from README) |
| 3 | Ensure `workflow-gates.mdc` small-run section matches path A/B/C merge-ready rules |
| 4 | Extend checklist orchestration appendix with path table (from [`../templates/universal-validation/validation-checklist.md`](../templates/universal-validation/validation-checklist.md)) |
| 5 | Add stable `flags` names to orchestration guide if the project extends the list |

---

## Human input (pause points)

Jarvis must **stop and ask** before:

| Decision | Why |
| --- | --- |
| Init path when signals **conflict** (e.g. user says “small” but requests full agent + orchestration scaffold) | Wrong path wastes sessions or leaves gaps |
| **Small** init when README claims heavy boundaries but user refuses ADRs | Handoff may fail `PROJ-HANDOFF-*` |
| **Skipping Validator** on architecture-touching or boundary-touching work | Violates default policy; needs explicit approval + `flags` |
| **Path C** when automated tests exist for the change | Path A is preferred |
| **Skipping Planner** on `large` or ADR-governed work | Defaults to medium+ |
| Treating **handoff complete** as waiver for **MG-*** on a feature run | Separate vocabularies ([`gates-and-checks.md`](./gates-and-checks.md)) |
| Changing init path or `{project-slug}` **after** the user locked them | Breaks task id continuity |
| Committing filled task folders to git when team default is local-only | [`task-folder-model.md`](./task-folder-model.md) § Git policy |

**Defaults without asking** (notify user, do not block):

- Init path unclear → **medium** ([`scaffolding-map.md`](../target-readme/scaffolding-map.md))
- Run tier unclear for a bounded fix → **small** path **A** if tests exist; else ask if architecture touched
- Mandatory universal scaffolds per init path → follow [A.3](#a3-proj--scope-matrix-by-init-path) until [`open-decisions.md`](../roadmap/open-decisions.md) closes

Routine copy of `_template/`, manifest updates per Orchestrator contract, and aligning checklist appendix keys do **not** require extra approval.

---

## Decisions recorded for `JR-ORCH-005`

| Topic | Decision |
| --- | --- |
| Canonical platform doc | This file — `init-paths.md` |
| Init vs run tier | **Two vocabularies** — init path (project once) vs `risk_tier.level` (per run) |
| Default init path | **Medium** when unclear |
| Default run tier for features | **Medium**; **small** only when B.2 elevation does not apply |
| Small run paths | **A** (B+T, no V), **B** (B+V, no T, non-testable), **C** (B only, trivial) |
| Gate 6 | Never skipped for code-changing runs |
| Full orchestrated merge-ready | **Yes** on medium, large, and small **B** when **MG-*** green; **No** on small **A** and **C** without human substitute for **MG-03** |
| Validator skip | Never on architecture/boundary work without explicit human approval |
| WFD source | Path A/B/C and tier tables — **no** WFD product/stack identifiers |
| Target duplication | INDEX and role contracts stay thin; matrices live in target orchestration guide adapted from this doc |
| Small init validation checklist | **Required** — minimal `validation-checklist.md` on all init paths (human decision 2026-05-19) |
| Medium init mandatory scaffolds | **Keep** § A.3 table defaults until [`open-decisions.md`](../roadmap/open-decisions.md) closes (human decision 2026-05-19) |

## Related

| Doc | Role |
| --- | --- |
| [`README.md`](./README.md) | Orchestration pack index |
| [`gates-and-checks.md`](./gates-and-checks.md) | Three vocabularies; small-run summary |
| [`task-folder-model.md`](./task-folder-model.md) | Paths, git policy |
| [`artifact-ownership.md`](./artifact-ownership.md) | When `test-matrix.md` is required |
| [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) | Init path entry point |
| [`../templates/universal-agents/orchestrator.example.md`](../templates/universal-agents/orchestrator.example.md) | Orchestrator rules 18–20 (paths) |
