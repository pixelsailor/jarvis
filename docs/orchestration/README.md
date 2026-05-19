# Universal orchestration scaffold

Jarvis guidance for **target-project** multi-agent runs: durable **task folders**, a **manifest-owned** pipeline, and stage artifacts that survive context loss. Inspired by What's For Dinner (WFD) discipline — **not** WFD product, stack, or identifiers.

**Platform tasks:** `JR-ORCH-001` (task-folder model), `JR-ORCH-002` (`task-manifest.json` shape), `JR-ORCH-003` (artifact ownership)  
**Follow-on (not in this pack):** `JR-ORCH-004`–`007` (gates vs merge-ready, init paths, loops, self-containment). **Agents:** [`../universal-agents/README.md`](../universal-agents/README.md) — `JR-AGENT-001` (INDEX), `JR-AGENT-002` (role contracts); `JR-AGENT-003`–`004` (minimum read sets, handoff prompts).

**Read order for agents (Jarvis initializing orchestration in a target):**

1. Target root `README.md` — initialization path (large init usually needs orchestration)
2. [`task-folder-model.md`](./task-folder-model.md) — where runs live, artifact set, bootstrap, git policy
3. [`artifact-ownership.md`](./artifact-ownership.md) — who writes each file, section contracts, freeze/loop rules
4. [`task-manifest.md`](./task-manifest.md) — JSON field contract and enums
5. [`../templates/orchestration/`](../templates/orchestration/) — copy `_template/` into the target
6. [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) — `PROJ-ORCH-*` backlog rows
7. [`../universal-validation/README.md`](../universal-validation/README.md) — checklist + orchestration appendix when validation doc exists

## Workflow documents

| ID | Document | Use when |
| --- | --- | --- |
| `JR-ORCH-001` | [`task-folder-model.md`](./task-folder-model.md) | Choosing paths, artifacts, lifecycle vocabulary, bootstrap, git policy |
| `JR-ORCH-002` | [`task-manifest.md`](./task-manifest.md) | Creating or validating `task-manifest.json`; schema migrations via `manifest_version` |
| `JR-ORCH-003` | [`artifact-ownership.md`](./artifact-ownership.md) | Per-file writers, required sections, freeze and loop behavior |
| (target templates) | [`../templates/orchestration/_template/`](../templates/orchestration/_template/) | Manifest example + seven markdown stage templates |

## Target layout (after copy)

| Path | Role |
| --- | --- |
| `.cursor/orchestrations/_template/` | Read-only templates; copy into `{task-id}/` per run |
| `.cursor/orchestrations/{task-id}/` | One folder per run; folder slug **must equal** manifest `task_id` |
| `.cursor/agents/INDEX.md` | Agent roster — copy from [`../templates/universal-agents/`](../templates/universal-agents/) (`JR-AGENT-001`) |
| `.cursor/agents/*.md` | Per-role contracts — copy from [`../templates/universal-agents/`](../templates/universal-agents/) (`JR-AGENT-002`) |
| `docs/ORCHESTRATED_DEVELOPMENT.md` (or project-chosen name) | Human orchestration guide — adapt from WFD pattern, target-owned |

Jarvis does **not** copy WFD's `docs/ORCHESTRATED_DEVELOPMENT.md` verbatim. Targets author their guide after templates and agent contracts exist.

## Human input (pause points)

Jarvis must **stop and ask** before:

- Using a task-folder root **other than** `.cursor/orchestrations/` (team policy or monorepo layout).
- Changing the **`{project-slug}-NNN`** convention after the user set `project-slug` at init (e.g. switching to `orch-NNN`).
- **Committing** filled task folders to git when the team default is local-only (see [`task-folder-model.md` § Git policy](./task-folder-model.md#git-policy)).
- **Skipping** Planner or Validator on architecture-touching work without explicit user approval and manifest rationale.
- Replacing an existing target orchestration layout the team already adopted.

Routine copy of `_template/`, setting `task_id` / `objective`, and manifest updates per Orchestrator contract do not require extra approval.

## Decisions recorded for `JR-ORCH-001` / `JR-ORCH-002`

Defaults favor long-term agent efficiency; override per target when the user directs:

| Topic | Decision |
| --- | --- |
| Task-folder root | **`.cursor/orchestrations/{task-id}/`** |
| `task_id` / folder slug | **`{project-slug}-NNN`** (e.g. `acme-001`); slug chosen once at init; folder name **must match** manifest `task_id` exactly |
| Durable state | Task folder is the record; do not use repo-root ad-hoc `_*.md` for run state |
| Manifest owner | **Orchestrator only** (other roles read-only) |
| `pipeline` array | Stage handoff agents only (`planner` → `builder` → `tester` → `validator`); Orchestrator bookends intake and Gate 6 — **not** listed in `pipeline` |
| Schema richness | **Full** WFD-shaped manifest (gates 0–6, loops, rework, `session_counts`, `command_evidence`, `flags`, `human_approval`) |
| `manifest_version` | **`1.0.0`** for Jarvis universal template; targets may bump when forking schema — document in target orchestration guide |
| Product/stack in manifest | **Never** — no WFD IDs, package managers, or provider names in the generic template |

## Decisions recorded for `JR-ORCH-003`

| Topic | Decision |
| --- | --- |
| Canonical contract | [`artifact-ownership.md`](./artifact-ownership.md) — section order, freeze, loop behavior |
| Template pack | Manifest example + `plan.md`, `acceptance-criteria.md`, `test-matrix.md`, `build-log.md`, `test-report.md`, `validation-report.md`, `human-approval.md` |
| Planner freeze | After `gate_2_plan` → `passed`, Planner artifacts immutable until rework to Planner |
| Build log on loops | **Append** dated sections |
| Test / validation reports | **Replace** each stage invocation |
| Gate 6 mirror | `human-approval.md` must match manifest `human_approval` |
| AC IDs | `AC-NN` zero-padded |

## Related material

| Resource | Role |
| --- | --- |
| [`../roadmap/platform-spec.md`](../roadmap/platform-spec.md) | WFD concepts to generalize; `PROJ-ORCH-*` prefix |
| [`../universal-validation/README.md`](../universal-validation/README.md) | **MG-** merge-ready vs lifecycle gates |
| [`../templates/universal-validation/validation-checklist.md`](../templates/universal-validation/validation-checklist.md) | Orchestration appendix (lifecycle ↔ checklist) |
| WFD reference (conceptual) | Task-folder model in a mature project — do not link from target artifacts |
