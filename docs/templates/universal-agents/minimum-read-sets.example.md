# Minimum read sets (template)

> **Template** — Copy the sections below into `.cursor/agents/INDEX.md` under **## Minimum read sets**, or into your orchestration guide appendix. Replace `REPLACE_WITH_*` paths. Delete this callout block in the target file. Do not list Jarvis or platform task IDs in target artifacts.

Efficiency tables complement mandatory **Inputs** in each `.cursor/agents/*.md` contract — do not omit contract inputs because a path is absent from these tables.

**Orchestration guide (policy detail):** [`REPLACE_WITH_ORCHESTRATION_GUIDE_PATH`](../../REPLACE_WITH_ORCHESTRATION_GUIDE_PATH)

## Project baseline (any agent)

| Always read | Read when applicable |
| --- | --- |
| Root `README.md` | `docs/roadmap/README.md`, `docs/roadmap/backlog.md` (open `PROJ-*` for this work) |
| | `adrs/INDEX.md` when architecture may change |
| | `.cursor/rules/index.md` when editing code or rules |

## Orchestrator

| Always read | Read when applicable |
| --- | --- |
| `.cursor/orchestrations/{task-id}/task-manifest.json` | Stage outputs listed in `completed_stages[].output_artifacts` |
| `.cursor/agents/orchestrator.md` | `validation-report.md` after Validator |
| [`REPLACE_WITH_ORCHESTRATION_GUIDE_PATH`](../../REPLACE_WITH_ORCHESTRATION_GUIDE_PATH) — routing, skips, loops, Gate 6 | `human-approval.md` on Gate 6 / rework resume |
| | `plan.md` for Gate 1–2 or open-question checks |
| | `build-log.md`, `test-report.md` for Gate 6 evidence |

## Planner

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json` | Accepted ADRs (full files) cited or plausibly relevant |
| `adrs/INDEX.md`, `adrs/GOVERNANCE.md` | Source paths for file map |
| `.cursor/orchestrations/_template/plan.md`, `acceptance-criteria.md` | [`REPLACE_WITH_VALIDATION_CHECKLIST_PATH`](../../REPLACE_WITH_VALIDATION_CHECKLIST_PATH) — applicable sections |
| [`REPLACE_WITH_COMMANDS_DOC`](../../REPLACE_WITH_COMMANDS_DOC) | [`REPLACE_WITH_TEST_MATRIX_PATH`](../../REPLACE_WITH_TEST_MATRIX_PATH) or `docs/stack/testing-strategy.md` when matrix required |

## Builder

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json` (`locked_artifacts`) | ADRs cited in `plan.md` |
| `plan.md`, `acceptance-criteria.md` | Files in plan file map; topic rules for touched paths |
| | Prior `build-log.md` + `validation-report.md` (**Required remediations**) on remediation loops |

## Tester

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json` | `test-matrix.md` (**Planned coverage**) when required |
| `acceptance-criteria.md`, `build-log.md` | `plan.md` (context only) |
| Tests near changed paths | [`REPLACE_WITH_TEST_MATRIX_PATH`](../../REPLACE_WITH_TEST_MATRIX_PATH) / `docs/stack/testing-strategy.md` |

## Validator

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json` | `test-matrix.md` when used |
| `plan.md`, `acceptance-criteria.md`, `build-log.md`, `test-report.md` | ADRs cited in `plan.md`; files named in artifacts |
| [`REPLACE_WITH_VALIDATION_CHECKLIST_PATH`](../../REPLACE_WITH_VALIDATION_CHECKLIST_PATH) — applicable sections | Project alignment-gaps doc if referenced in plan/ADRs |
| `.cursor/agents/validator.md` + workflow-gates rule | |

## Session variants (quick reference)

| Variant | Adjustment |
| --- | --- |
| Validator **FAIL** → Builder | Add `validation-report.md` **Required remediations**; append `build-log.md` |
| Human **rejected_rework** | Orchestrator reads approval record; Planner if scope changed, else Builder remediation set |
| Skipped Planner | Builder uses manifest `objective` + Orchestrator directive; read ADRs only if objective requires |
| Skipped Tester (small Path B) | Validator leans on `build-log.md` + documented **MG-04** substitute |
| Skipped Validator (small Path A) | Gate 6 uses `test-report.md` + human checklist substitute — no `validation-report.md` |

Tier and path-letter detail: [`REPLACE_WITH_ORCHESTRATION_GUIDE_PATH`](../../REPLACE_WITH_ORCHESTRATION_GUIDE_PATH).
