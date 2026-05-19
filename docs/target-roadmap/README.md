# Target project roadmap workflow

Jarvis uses the target project's `docs/roadmap/` folder as the **resumable setup backlog** after the root README exists. Tasks use stable `PROJ-*` IDs so agents can stop and resume without chat memory.

**Read order for agents (cold session on a target project):**

1. Target root `README.md`
2. `docs/roadmap/README.md` and `docs/roadmap/backlog.md`
3. [`conventions.md`](./conventions.md) when creating, editing, or completing backlog rows

**Platform context:** Terminology, default layout, and handoff expectations live in [`../roadmap/platform-spec.md`](../roadmap/platform-spec.md). Jarvis platform tasks use `JR-*`; target setup tasks use `PROJ-*`.

## Workflow documents

| ID | Document | Use when |
| --- | --- | --- |
| `JR-TARGET-TODO-001` | [`platform-spec.md`](../roadmap/platform-spec.md#default-location-and-layout) | Choosing filename, folder layout, and `PROJ-*` prefix |
| `JR-TARGET-TODO-002` | [`conventions.md`](./conventions.md) | Markdown structure, sections, task line shape, agent edit rules |
| `JR-TARGET-TODO-003` | [`conventions.md` § Task fields](./conventions.md#task-fields) | Required vs optional fields: ID, status, dependency, owner, evidence, blocker |
| `JR-TARGET-TODO-004` | [`readme-sync.md`](./readme-sync.md) | After any **material** target README edit; backlog add/reopen/defer/cancel |
| `JR-TARGET-TODO-005` | [`handoff.md`](./handoff.md) | Before Jarvis declares initialization complete |

## Related material

| Resource | Role |
| --- | --- |
| [`../templates/target-project-roadmap/`](../templates/target-project-roadmap/) | Copy-paste `README.example.md` and `backlog.example.md` into the target project |
| [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) | README signals → initial `PROJ-*` rows |
| [`../target-readme/handoff-checklist.md`](../target-readme/handoff-checklist.md) | README self-containment before `PROJ-HANDOFF-*` |
| [`../universal-handoff/README.md`](../universal-handoff/README.md) | Target `docs/handoff-self-containment.md` for `PROJ-HANDOFF-001` / `PROJ-HANDOFF-002` evidence |
| [`../stack-scaffolding/README.md`](../stack-scaffolding/README.md) | Stack detect/confirm, verified commands ([`commands.md`](../stack-scaffolding/commands.md)), then `PROJ-STACK-*` tooling tasks |

## Human input (pause points)

Jarvis must **stop and ask** before:

- Replacing an existing target `docs/roadmap/` with a parallel backlog (merge instead; see [`conventions.md`](./conventions.md#existing-roadmaps)).
- Deleting or renumbering `PROJ-*` IDs that already appear in ADRs, rules, or orchestration artifacts.
- Marking handoff complete while open **required for handoff** tasks remain (unless the user explicitly accepts partial handoff per [`handoff.md`](./handoff.md#partial-handoff-exception)).

Routine backlog rows, checkbox updates, and evidence sub-bullets that follow [`conventions.md`](./conventions.md) do not require extra approval.
