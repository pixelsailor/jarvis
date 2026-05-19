# Validation checklist — REPLACE_WITH_PROJECT_NAME

Cross-cutting audit checklist for **orchestrated Validator runs** (when the project defines them), **human PR review**, and **Planner scoping** (`plan.md` → Validation commands). It complements per-task `acceptance-criteria.md` with product and architecture gates from **Accepted** ADRs and project rules.

**Related artifacts:** `docs/pr-and-commit-guide.md`, [`docs/adr-alignment-gaps.md`](./adr-alignment-gaps.md), [`adrs/INDEX.md`](../../adrs/INDEX.md), [`.cursor/rules/index.md`](../../.cursor/rules/index.md). When orchestration exists: `validation-report.md`, `test-report.md`, and the project's orchestration guide.

After initialization, this repository must **not** depend on Jarvis or any external scaffolding repository to interpret this checklist.

---

## How to use

| Context | Action |
| --- | --- |
| **Orchestrated Validator** | Read this file; mark each **applicable** item in `validation-report.md` → **Checklist audit** (✅ / ⚠️ / ❌ / N/A + evidence). Any ❌ on a **required** item blocks **PASS** and **PASS_WITH_NOTES** per project orchestration policy. |
| **Planner** | Pull relevant rows into `plan.md` → **Validation commands**; map to AC IDs; do not paste the entire checklist unless the task is broad. |
| **Human PR review** | Use [`docs/pr-and-commit-guide.md`](./pr-and-commit-guide.md) and the PR template; copy applicable checklist sections into review comments; link alignment gaps when deviating. |

**Evidence** means file:line, test name, command output, or a named manual step (for example “smoke: primary flow loads with network disabled in DevTools”).

---

## Applicability

Run a section when the change **touches** that domain. When unsure, include the section and mark non-applicable items **N/A** with a one-line justification.

| Section | Run when change involves… |
| --- | --- |
| [Merge-ready gates](#merge-ready-gates) | Any architecture-touching PR; orchestrated work when defined |
| [Tests and evidence](#tests-and-evidence) | All production changes (minimum bar below) |
| [Stack-specific extensions](#stack-specific-extensions) | Boundaries listed in README § Boundaries or filled extension table |
| [ADR-specific annexes](#adr-specific-annexes) | Domains where an **Accepted** ADR requires repeatable checks |
| [Orchestration appendix](#orchestration-appendix) | Only when the project uses task folders and lifecycle gates |

---

## Merge-ready gates

Binding for **architecture-touching** and **orchestrated** work when the project documents merge-ready policy (for example in `adrs/GOVERNANCE.md`, a workflow rule, or orchestration guide). For trivial or non-architectural PRs, use the generic checklist in `docs/pr-and-commit-guide.md` §4 instead of this section.

- [ ] **MG-01:** **Accepted** ADRs cited in `plan.md` (or PR/ADR list when Planner was skipped) were read; implementation matches or deviation is documented in [`docs/adr-alignment-gaps.md`](./adr-alignment-gaps.md) (or the path named in `adrs/GOVERNANCE.md`).
- [ ] **MG-02:** Durable architecture change has an ADR create/update **before** merge, or an explicit follow-up task with timeline (not comment-only).
- [ ] **MG-03:** `validation-report.md` exists with verdict, AC audit, ADR compliance, and **Checklist audit** when Validator stage ran (**N/A** only with documented substitute audit per orchestration policy).
- [ ] **MG-04:** `test-report.md` exists with AC coverage map and commands run when Tester stage ran (**N/A** when Tester skipped — PR must still carry equivalent test evidence).
- [ ] **MG-05:** Verified check/lint/test commands pass for touched paths: `REPLACE_WITH_VERIFY_COMMANDS` — or pre-existing failures outside scope are documented with evidence.

---

## Tests and evidence

**Refs:** Project test guide, `package.json` / manifest scripts, orchestration `test-matrix.md` when used.

- [ ] **TST-01:** Every AC in `acceptance-criteria.md` is mapped in `test-report.md` to a test, command, or explicit “untested” reason.
- [ ] **TST-02:** New logic has focused automated tests where the stack provides a practical runner; gaps recorded as residual risk in `test-report.md` / `test-matrix.md`.
- [ ] **TST-03:** Critical-path smoke is documented when the change affects availability, auth, or data paths called out in **Accepted** ADRs (automated or named manual step).
- [ ] **TST-04:** Commands listed in `test-report.md` were run or the report states why not (environment limitation).
- [ ] **TST-05:** When `test-matrix.md` exists, **Actual coverage** matches **Planned coverage** or gaps are listed in **Gaps vs plan** / `test-report.md` **Uncovered criteria**.

---

## Stack-specific extensions

Add rows when README boundaries or **Accepted** ADRs require repeatable checks beyond generic **MG-** / **TST-** rows. Remove example rows you do not use. Prefer one subsection per theme (for example **Security**, **Data**, **Auth**) when more than ~5 rows share the same refs.

| ID | Check | Refs |
| --- | --- | --- |
| _Example — delete or replace_ | **SEC-01:** Secrets and API keys stay server-side; not committed or exposed in client bundles | ADR / rule path |
| _Example — delete or replace_ | **AUTH-01:** Auth guards match documented session model; logged-out users retain documented local behavior | ADR / rule path |
| _Example — delete or replace_ | **DATA-01:** System of record for user-owned data matches ADR; no silent data loss on enhancement failure | ADR / rule path |
| _Example — delete or replace_ | **API-01:** External boundaries validate input/output; invalid payloads do not reach stores as authoritative | ADR / rule path |
| _Example — delete or replace_ | **UI-01:** New/changed UI meets project accessibility conventions (keyboard, names, landmarks) | ADR / rule path |
| _Example — delete or replace_ | **DEPLOY-01:** Runtime and env boundaries respected (no Node-only APIs in constrained handlers unless isolated) | ADR / rule path |
| _Example — delete or replace_ | **TOOL-01:** `REPLACE_WITH_VERIFY_COMMANDS` documented in README § Development match what CI/agents run | README § Development |

**Prefix guidance (choose per project):** `SEC-` security/secrets; `AUTH-` authentication; `DATA-` persistence/sync authority; `API-` HTTP or RPC boundaries; `UI-` / `A11Y-` presentation; `DEPLOY-` runtime/deployment; `TOOL-` lint/typecheck/test commands. Keep IDs stable once referenced in ADRs or orchestration artifacts.

---

## ADR-specific annexes

When an **Accepted** ADR needs checklist items that do not belong in a generic extension row, add a subsection here. Name the subsection after the ADR topic; use a short prefix (for example `PAY-01` for payments ADR).

### _ADR title placeholder (ADR-NNN)_

- [ ] **_PREFIX_-01:** _Replace with a testable statement tied to the ADR decision._
- [ ] **_PREFIX_-02:** _Optional second row._

_Delete this subsection until the first annex is required._

---

## Orchestration appendix

**Enable when** the project scaffolds orchestration (`task-manifest.json`, lifecycle gates, `validation-report.md` template). Until then, leave this section as reference or delete it.

Orchestrated projects use **three gate vocabularies** — do not conflate them (narrative in the project orchestration guide, adapted from Jarvis `gates-and-checks` discipline):

| Vocabulary | Typical range | Tracks | Recorded in |
| --- | --- | --- | --- |
| **Lifecycle gates** | Project-defined (e.g. 0–6) | Pipeline stage completion | `task-manifest.json` → `gate_status` (keys per project orchestration doc) |
| **Merge-ready checks** | **MG-01**–**MG-05** above | Whether work may be claimed **merge-ready** | Checklist + `validation-report.md` / PR; `.cursor/rules/workflow-gates.mdc` |
| **Handoff checks** | **PROJ-HANDOFF-*** | Target repo independent of scaffolding repos | `docs/handoff-self-containment.md`, `docs/roadmap/backlog.md` |

**Lifecycle Gate 5** (validation green) ≠ **MG-05** (tooling). **Gate 6** (human approval) follows a green merge-ready path. Handoff completion ≠ merge-ready for a feature run.

### Lifecycle gate ↔ checklist (fill per project)

| Lifecycle gate | `gate_status` key _(example)_ | Primary owner | Checklist / merge-ready |
| --- | --- | --- | --- |
| _Intake_ | `gate_0_intake` | Orchestrator | Risk tier; skipped stages documented |
| _Requirements_ | `gate_1_requirements` | Planner | `acceptance-criteria.md`; feeds **TST-01** |
| _Plan_ | `gate_2_plan` | Planner | `plan.md` ADR list → **MG-01** / **MG-02** |
| _Build_ | `gate_3_build` | Builder | `build-log.md`; supports **MG-05** |
| _Tests_ | `gate_4_tests` | Tester | **MG-04**, **TST-*** |
| _Validation_ | `gate_5_validation` | Validator | **MG-03** + applicable domain rows |
| _Human approval_ | `gate_6_human_approval` | Orchestrator | Not checklist IDs; human sign-off artifact |

Domain sections (**SEC-**, **AUTH-**, stack extensions, annex rows) are usually audited at the **validation** lifecycle stage when applicable — not as separate lifecycle gates.

### Small-run skips

When the project defines a **small** risk tier with skipped Planner / Tester / Validator stages, document skips in the manifest with rationale. **Do not** claim full orchestrated merge-ready without substitute evidence (human review + PR checklist) when Validator was skipped on architecture- or boundary-touching work.

### Non-orchestrated PRs

No `gate_status`. Use **MG-01**, **MG-02**, **MG-05** plus PR test evidence (**MG-04** style). **MG-03** applies only when `validation-report.md` exists for that change.

---

## Checklist versioning

| Version | Date | Notes |
| --- | --- | --- |
| 1.0.0 | _YYYY-MM-DD_ | Initial checklist from Jarvis universal template: MG-*, TST-*, stack extensions, optional orchestration appendix |

When checklist items change, bump this table and sync Validator output contract / orchestration templates if IDs or required sections change.
