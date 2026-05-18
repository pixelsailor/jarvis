# Rules index — REPLACE_WITH_PROJECT_NAME

Catalog of **Cursor rules** (`.cursor/rules/*.mdc`). Read this file when choosing which rule to load for a task.

**Agent contracts** (orchestration roles) are **not** Cursor rules. When present, see [`.cursor/agents/INDEX.md`](../agents/INDEX.md) or [`agents/INDEX.md`](../../agents/INDEX.md) — adjust path to match this repository.

## Lookup by category

| Category | Rule | One-liner | Activation |
| --- | --- | --- | --- |
| **Routing** | [project-routing.mdc](./project-routing.mdc) | Repo layout; rule routing table; where ADRs and docs live | alwaysApply |
| **Governance** | [adr-compliance.mdc](./adr-compliance.mdc) | Consult **Accepted** ADRs before build work; record alignment gaps | alwaysApply |
| **Governance** | readme-governance | Root README scope; route depth to `docs/readme-governance.md` | globs `README.md` — add when [`readme-governance.mdc`](./readme-governance.mdc) and [`docs/readme-governance.md`](../../docs/readme-governance.md) exist |
| **ADR-backed** | _—_ | Boundary enforcement rules cite ADR IDs in `description` and body | globs per rule — add from `PROJ-RULE-*` |
| **Stack** | _—_ | Framework, lint, test, or MCP playbooks | globs per rule — add from `PROJ-STACK-*` |
| **Workflow** | _—_ | PR/commit, validation checklist, orchestration gates | alwaysApply or globs — add when those scaffolds exist |

Remove placeholder rows as you add real rules. Keep **one row per `.mdc` file**.

## Activation modes

| Mode | Behavior | Rules (update as project grows) |
| --- | --- | --- |
| **alwaysApply** | Loaded every agent session | project-routing, adr-compliance |
| **globs** | Loaded when matching files are in context | _(none yet)_ |
| **manual** | Invoked only when the user names the rule | _(none)_ |

## Maintenance

- Add or update a row in **the same session** as any new or changed `.mdc` file.
- Prefer [`docs/rules-authoring.md`](../../docs/rules-authoring.md) when authoring or splitting rules (large-path projects).
- Do not duplicate ADR bodies in rules — link `adrs/ADR-NNN-*.md` and keep enforcement concise.
