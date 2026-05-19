# Orchestration task templates

Copy these files into **`.cursor/orchestrations/{task-id}/`** when starting a new run. The directory name and manifest `task_id` **must match exactly** (replace `REPLACE_WITH_PROJECT_SLUG-000` with your slug, e.g. `acme-001`).

**Jarvis source:** Universal orchestration templates — adapt filenames and contracts in the target orchestration guide.  
**Schema:** [`docs/orchestration/task-manifest.md`](../../../orchestration/task-manifest.md) (path after copy: target-owned doc).

## Files in this template pack (`JR-ORCH-001` / `002`)

| File | Owner | When |
| --- | --- | --- |
| `task-manifest.example.json` → `task-manifest.json` | Orchestrator | Bootstrap run — set `task_id`, `objective`, `locked_artifacts`, `risk_tier` |
| This README | — | Reference only; do not copy into `{task-id}/` |

Additional markdown artifacts (`plan.md`, `acceptance-criteria.md`, `test-matrix.md`, `build-log.md`, `test-report.md`, `validation-report.md`, `human-approval.md`) are added when the target scaffolds full orchestration (`PROJ-ORCH-*`, `JR-ORCH-003`+).

## `gate_status` values

Each key (`gate_0_intake` … `gate_6_human_approval`): `pending` | `passed` | `failed` | `blocked` | `skipped` | `n/a`.

Use `skipped` only with rationale in `risk_tier.skipped_stages` and `risk_tier.rationale`.

## Git policy (default)

During the run, agents do **not** commit on their own. After Gate 6 (`human-approval.md` recorded, manifest `status: complete`), the Orchestrator asks whether to commit application changes and optionally `.cursor/orchestrations/{task-id}/`. Run `git commit` only when the human explicitly confirms.

Until then, keep task folders local and put **review evidence in the PR** (commands, verdict, AC coverage, MG-* checklist).

## Related (target repo, after scaffold)

- `docs/ORCHESTRATED_DEVELOPMENT.md` — human orchestration guide (name chosen by project)
- `.cursor/agents/INDEX.md` — role contracts (`JR-AGENT-*`)
- `docs/validation-checklist.md` — merge-ready **MG-*** and lifecycle mapping
