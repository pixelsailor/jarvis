# `task-manifest.json` shape (`JR-ORCH-002`)

Authoritative JSON contract for orchestration run state in target projects. **Only the Orchestrator** (or a human following the same contract) may create or update this file; other pipeline roles treat it as **read-only**.

**Folder model:** [`task-folder-model.md`](./task-folder-model.md)  
**Example file:** [`../templates/orchestration/_template/task-manifest.example.json`](../templates/orchestration/_template/task-manifest.example.json)

## File requirements

- Valid **JSON** (no comments in committed manifests).
- Lives at `.cursor/orchestrations/{task-id}/task-manifest.json`.
- `task_id` **must equal** the parent folder name.

## Top-level schema

| Field | Type | Required | Owner | Description |
| --- | --- | --- | --- | --- |
| `manifest_version` | string (semver) | yes | Orchestrator | Schema version for migrations; Jarvis universal template uses **`1.0.0`** |
| `task_id` | string | yes | Orchestrator | `{project-slug}-NNN`; matches folder slug |
| `objective` | string | yes | Orchestrator | Actionable success outcome for the run |
| `status` | enum | yes | Orchestrator | Run lifecycle — see [Status](#status) |
| `current_agent` | enum | yes | Orchestrator | Who acts next — see [Agents](#agents) |
| `pipeline` | string[] | yes | Orchestrator | Ordered stage agents **excluding** Orchestrator bookends |
| `risk_tier` | object | yes | Orchestrator | Tier and skip rationale — see [Risk tier](#risk-tier) |
| `gate_status` | object | yes | Orchestrator | Lifecycle gates 0–6 — see [Gate status](#gate-status) |
| `completed_stages` | array | yes | Orchestrator | Short factual summaries per finished stage |
| `loop_count` | number | yes | Orchestrator | Validator **FAIL** remediation loops — increment **only** when routing to Builder; see [`loops-and-rework.md`](./loops-and-rework.md) |
| `max_loops` | number | yes | Orchestrator | Cap before `blocked` (default **2** unless project doc overrides) |
| `rework_count` | number | yes | Orchestrator | Human **rejected_rework** cycles — separate from `loop_count` |
| `rework_history` | array | yes | Orchestrator | Entries for each rework — see [Rework history](#rework-history) |
| `session_counts` | object | yes | Orchestrator | Invocations per agent role — see [Session counts](#session-counts) |
| `command_evidence` | array | yes | Orchestrator | Commands run / skipped — see [Command evidence](#command-evidence) |
| `locked_artifacts` | string[] | yes | Orchestrator | Repo paths treat as read-only for downstream roles |
| `flags` | string[] | yes | Orchestrator | Machine-readable tags (skips, escalations, ADR conflicts) |
| `human_approval` | object | yes | Orchestrator | Gate 6 state — see [Human approval](#human-approval) |

### Status

| Value | Meaning |
| --- | --- |
| `in_progress` | Pipeline active |
| `awaiting_human` | Validation path green (or small-run substitute); Gate 6 pending |
| `complete` | Human approved; run closed |
| `blocked` | Failure, max loops, rejection, or unresolved contradiction |

### Agents

Values for `current_agent` and entries in `pipeline`:

| Value | Role |
| --- | --- |
| `orchestrator` | Intake, routing, Gate 6, loops |
| `planner` | Plan + acceptance criteria (+ planned test matrix) |
| `builder` | Implementation + build log |
| `tester` | Tests + test report |
| `validator` | Validation report + checklist audit |

`pipeline` example:

```json
["planner", "builder", "tester", "validator"]
```

Do **not** include `orchestrator` in `pipeline`.

## Risk tier

```json
"risk_tier": {
  "level": "small | medium | large",
  "rationale": "string",
  "skipped_stages": ["planner", "tester"]
}
```

| `level` | Use |
| --- | --- |
| `small` | Minimal scope; may skip stages with documented rationale — paths **A** / **B** / **C** in [`init-paths.md`](./init-paths.md) |
| `medium` | Default feature work; full pipeline unless justified |
| `large` | Cross-cutting / ADR-heavy; Planner required |

`skipped_stages` lists **pipeline role names** skipped (not gate keys). Each skip must align with `gate_status` `skipped` and narrative in `rationale` or `flags`. Full tier and skip matrices: [`init-paths.md`](./init-paths.md) (`JR-ORCH-005`).

## Gate status

Fixed keys (lifecycle **0–6**):

| Key | Gate | Typical owner |
| --- | --- | --- |
| `gate_0_intake` | Intake and risk tier | Orchestrator |
| `gate_1_requirements` | Requirements freeze | Planner |
| `gate_2_plan` | Executable plan | Planner |
| `gate_3_build` | Build complete | Builder |
| `gate_4_tests` | Tests mapped | Tester |
| `gate_5_validation` | Validation green | Validator |
| `gate_6_human_approval` | Human approval | Orchestrator |

**Per-key values:**

| Value | Meaning |
| --- | --- |
| `pending` | Not started |
| `passed` | Stage satisfied |
| `failed` | Stage failed; may trigger loop or block |
| `blocked` | Cannot proceed until external input |
| `skipped` | Intentionally skipped (requires `risk_tier` rationale) |
| `n/a` | Not applicable to this run |

## Session counts

Object keys match agent roles. Increment when that role is invoked for the run (Orchestrator typically starts at `1` for itself).

```json
"session_counts": {
  "orchestrator": 1,
  "planner": 0,
  "builder": 0,
  "tester": 0,
  "validator": 0
}
```

## Command evidence

Array of objects summarizing commands the pipeline relied on (Orchestrator aggregates from stage artifacts):

```json
{
  "command": "string — exact script from package.json or equivalent",
  "agent": "builder | tester | validator | orchestrator",
  "result": "pass | fail | skipped",
  "notes": "optional string"
}
```

Use **verified** commands from the target manifest only — never invent scripts ([`../stack-scaffolding/commands.md`](../stack-scaffolding/commands.md)).

## Rework history

Append on human `rejected_rework` or scope-changing rework:

```json
{
  "at": "ISO-8601 timestamp",
  "reason": "string",
  "routed_to": "builder | planner"
}
```

## Human approval

```json
"human_approval": {
  "status": "pending | approved | approved_with_conditions | rejected | rejected_rework",
  "approved_at": "ISO-8601 string or null",
  "approver": "string or null",
  "conditions": "string or null",
  "notes": "string or null"
}
```

Gate 6 must also be recorded in `human-approval.md` with the same facts — see [`artifact-ownership.md`](./artifact-ownership.md) § `human-approval.md`.

## Flags

Free-form string tags for routing and audits. Examples (targets may extend):

- `small_run_path_a` | `small_run_path_b` | `small_run_path_c`
- `skipped_validator_small_run`
- `skipped_tester_non_testable`
- `substitute_audit_owner: human`
- `adr_conflict`
- `objective_blocked`

Prefer stable flag names documented in the target orchestration guide.

## Completed stages

Array of short strings, e.g. `"planner: plan.md + AC-01..AC-05"`. Orchestrator appends when a stage finishes; do not duplicate full artifact contents.

## Locked artifacts

Repo-relative paths (files or directories) downstream agents must not modify without Orchestrator unlock. Example: `["adrs/ADR-012-client-boundary.md"]`.

## Minimal valid example

See [`task-manifest.example.json`](../templates/orchestration/_template/task-manifest.example.json).

## Schema evolution

1. Bump `manifest_version` (semver) when adding required fields or changing enums.
2. Document migration in the target orchestration guide.
3. Orchestrator may rewrite manifest on resume only per migration notes — do not let Builder/Tester/Validator edit the file.

Jarvis universal template **`1.0.0`** matches the full field set in this document. Projects ported from WFD `2.0.0` manifests should treat `manifest_version` as target-owned once forked.

## Validation checklist (agents)

Before handoff:

- [ ] JSON parses; all required keys present
- [ ] `task_id` equals parent directory name
- [ ] `pipeline` has no `orchestrator` entry
- [ ] Every `skipped` gate has `risk_tier.skipped_stages` / rationale alignment
- [ ] `current_agent` matches the next role to invoke
- [ ] `human_approval` mirrors `human-approval.md` when Gate 6 recorded

## Related

| Doc | Role |
| --- | --- |
| [`task-folder-model.md`](./task-folder-model.md) | Paths, artifacts, git policy |
| [`artifact-ownership.md`](./artifact-ownership.md) | Stage file contracts |
| [`loops-and-rework.md`](./loops-and-rework.md) | When counters increment; remediation vs rework routing |
| [`README.md`](./README.md) | Index |
