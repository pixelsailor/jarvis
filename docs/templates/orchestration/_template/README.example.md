# Orchestration task templates

Copy these files into **`.cursor/orchestrations/{task-id}/`** when starting a new run. The directory name and manifest `task_id` **must match exactly** (replace `REPLACE_WITH_PROJECT_SLUG-000` with your slug, e.g. `acme-001`).

**Jarvis source:** Universal orchestration templates — adapt contracts in the target orchestration guide.  
**Artifact ownership:** [`docs/orchestration/artifact-ownership.md`](../../../orchestration/artifact-ownership.md) (`JR-ORCH-003`).  
**Manifest schema:** [`docs/orchestration/task-manifest.md`](../../../orchestration/task-manifest.md) (`JR-ORCH-002`).

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
| This README | — | Reference only; do not copy into `{task-id}/` |

Copy all markdown templates at bootstrap; delete or leave empty sections roles do not use only when `risk_tier` and skip policy allow (document skips in manifest).

## `gate_status` values

Each key (`gate_0_intake` … `gate_6_human_approval`): `pending` | `passed` | `failed` | `blocked` | `skipped` | `n/a`.

Use `skipped` only with rationale in `risk_tier.skipped_stages` and `risk_tier.rationale`.

## Git policy (default)

During the run, agents do **not** commit on their own. After Gate 6 (`human-approval.md` recorded, manifest `status: complete`), the Orchestrator asks whether to commit application changes and optionally `.cursor/orchestrations/{task-id}/`. Run `git commit` only when the human explicitly confirms.

Until then, keep task folders local and put **review evidence in the PR** (commands, verdict, AC coverage, MG-* checklist).

## Related (target repo, after scaffold)

- `docs/ORCHESTRATED_DEVELOPMENT.md` — human orchestration guide (name chosen by project)
- `.cursor/agents/INDEX.md` — agent roster ([`universal-agents`](../../../universal-agents/) `JR-AGENT-001`)
- `.cursor/agents/orchestrator.md`, `planner.md`, `builder.md`, `tester.md`, `validator.md` — role contracts (`JR-AGENT-002`)
- `docs/validation-checklist.md` — merge-ready **MG-*** and lifecycle mapping
