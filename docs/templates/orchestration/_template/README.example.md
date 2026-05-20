# Orchestration task templates

> **Template** — After copy to `.cursor/orchestrations/_template/`, rename to `README.md`, replace placeholders below, and **delete this callout block**. Do not leave Jarvis or platform task IDs in the target file. Sanitization: Jarvis `docs/orchestration/self-containment.md` (`JR-ORCH-007`).

Copy these files into **`.cursor/orchestrations/{task-id}/`** when starting a new run. The directory name and manifest `task_id` **must match exactly** (replace `REPLACE_WITH_PROJECT_SLUG-000` with your slug, e.g. `acme-001`).

**Contracts:** Artifact sections and freeze rules — target orchestration guide ([`REPLACE_WITH_ORCHESTRATION_GUIDE_PATH`](../../REPLACE_WITH_ORCHESTRATION_GUIDE_PATH)) and `.cursor/agents/planner.md` (and sibling role contracts). Manifest field definitions — same guide or target manifest appendix.

## Files in this template pack

| File | Primary owner | When |
| --- | --- | --- |
| `task-manifest.example.json` → `task-manifest.json` | Orchestrator | Bootstrap — set `task_id`, `objective`, `locked_artifacts`, `risk_tier` |
| `plan.md` | Planner | Before Builder |
| `acceptance-criteria.md` | Planner | Before Builder |
| `test-matrix.md` | Planner (planned) → Tester (actual) | Medium/large or testable code change |
| `build-log.md` | Builder | Before Tester |
| `test-report.md` | Tester | Before Validator |
| `validation-report.md` | Validator | Before `awaiting_human` (when Validator runs) |
| `human-approval.md` | Orchestrator | Gate 6 — mirrors manifest `human_approval` |
| This README | — | Reference only in `_template/`; do not copy into `{task-id}/` |

Copy all markdown templates at bootstrap; delete or leave empty sections roles do not use only when `risk_tier` and skip policy allow (document skips in manifest).

## `gate_status` values

Each key (`gate_0_intake` … `gate_6_human_approval`): `pending` | `passed` | `failed` | `blocked` | `skipped` | `n/a`.

Use `skipped` only with rationale in `risk_tier.skipped_stages` and `risk_tier.rationale`.

## Git policy (default)

During the run, agents do **not** commit on their own. After Gate 6 (`human-approval.md` recorded, manifest `status: complete`), the Orchestrator asks whether to commit application changes and optionally `.cursor/orchestrations/{task-id}/`. Run `git commit` only when the human **requests** a pipeline commit and confirms in-thread; set `orchestrated_commit_requested` in manifest `flags` and follow `docs/pr-and-commit-guide.md`.

Until then, keep task folders local and put **review evidence in the PR** (commands, verdict, AC coverage, MG-* checklist).

## Related (this repository)

- [`REPLACE_WITH_ORCHESTRATION_GUIDE_PATH`](../../REPLACE_WITH_ORCHESTRATION_GUIDE_PATH) — human orchestration guide
- `.cursor/agents/INDEX.md` — agent roster
- `.cursor/agents/orchestrator.md`, `planner.md`, `builder.md`, `tester.md`, `validator.md` — role contracts
- [`REPLACE_WITH_VALIDATION_CHECKLIST_PATH`](../../REPLACE_WITH_VALIDATION_CHECKLIST_PATH) — merge-ready **MG-*** and lifecycle mapping (lifecycle, merge-ready, handoff — three vocabularies)
- `.cursor/rules/workflow-gates.mdc` — agent-facing merge-ready contract (when present)
