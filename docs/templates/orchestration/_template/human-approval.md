# Human approval — REPLACE_WITH_TASK_ID

Replace `REPLACE_WITH_TASK_ID` with `task_id`. Complete Gate 6 after the validation path is green (normally Validator **PASS** or **PASS_WITH_NOTES**) and before the run is marked `complete`.

## Evidence summary

| Field | Value |
| ----- | ----- |
| Task ID |  |
| Objective |  |
| Risk tier |  |
| Files changed |  |
| Acceptance coverage |  |
| Validation verdict |  |
| Commands run |  |
| Commands not run |  |
| Rework count |  |
| Known follow-ups |  |

## Approval decision

| Field | Value |
| ----- | ----- |
| Approver |  |
| Outcome | pending / approved / approved_with_conditions / rejected / rejected_rework |
| Approved at (ISO-8601) |  |
| Conditions |  |
| Notes |  |

## Rework directive

Complete only when the outcome is `rejected_rework`.

| Field | Value |
| ----- | ----- |
| Rejected by |  |
| Rejection timestamp (ISO-8601) |  |
| Rejection reason |  |
| Required rework |  |
| Return stage | Builder / Planner |

## Confirmation

I have reviewed the implementation, tests, validation report, and command evidence for this task and approve the recorded Gate 6 outcome.

**Signature / record:** (name or system ID)

---

Mirror the same outcome fields in `task-manifest.json` under `human_approval`. If the outcome is `rejected_rework`, increment `rework_count`, append `rework_history`, and route per the target orchestration guide (typically Builder → Tester → Validator; Planner when scope changes). Do **not** increment `loop_count` for human rework — that counter is for Validator **FAIL** remediation only.

When outcome is `approved` or `approved_with_conditions`, the Orchestrator asks whether to create a **pipeline** git commit (application changes; optionally this task folder). Commit only if the human explicitly requests and confirms in the same thread; set manifest flag `orchestrated_commit_requested` and follow `docs/pr-and-commit-guide.md` § Commit messages. If the human declines, the run may still complete without a pipeline commit.
