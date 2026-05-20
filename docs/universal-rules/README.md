# Universal Cursor rules scaffold

Generic `.cursor/rules/` layout and rules index for target projects. Jarvis copies and adapts these files so agents can find the narrowest rule without loading the full corpus every session.

**Platform task:** `JR-UNIVERSAL-004`  
**Copy source:** [`../templates/universal-rules/`](../templates/universal-rules/)  
**Target layout:**

| Path | Role |
| --- | --- |
| `.cursor/rules/index.md` | Catalog: categories, one-liners, activation modes (not a Cursor rule file) |
| `.cursor/rules/project-routing.mdc` | Always-apply: repo layout and rule routing table |
| `.cursor/rules/adr-compliance.mdc` | Always-apply: consult **Accepted** ADRs before build work |
| `docs/rules-authoring.md` | Optional on **large** path — how to add or change rules without duplicating ADRs |

Topic rules (`globs`, boundary enforcement, stack playbooks) are added per README boundaries and `PROJ-RULE-*` tasks — not shipped in the universal starter set.

**Rule discipline (platform canonical):**

| Doc | Task |
| --- | --- |
| [`activation-modes.md`](./activation-modes.md) | `JR-RULE-001` — alwaysApply vs globs vs manual |
| [`authoring.md`](./authoring.md) | `JR-RULE-002` — rule vs ADR vs doc, naming, workflow |
| [`adr-and-doc-citation.md`](./adr-and-doc-citation.md) | `JR-RULE-003` — Binding/Supporting, gaps, paths |
| _(open)_ | `JR-RULE-004` — documentation conventions as Cursor rules (blocked on reference material) |

## When to scaffold

| Initialization path | Cursor rules |
| --- | --- |
| **Small** | Optional — `project-routing.mdc` only if agents will edit code; skip index until a second rule exists |
| **Medium** | Default — `index.md` + `project-routing.mdc` + `adr-compliance.mdc`; link index from root README § Documentation |
| **Large** | Required — same as medium + `docs/rules-authoring.md`; populate index rows as boundary and stack rules land |

See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md).

## Copy and adapt (agents)

1. If the target already has `.cursor/rules/`, **audit** — merge into `index.md`; do not overwrite project-specific rule bodies without user approval.
2. Create `.cursor/rules/` if missing.
3. Copy [`index.md`](../templates/universal-rules/index.md) → `.cursor/rules/index.md`.
4. Copy [`project-routing.mdc`](../templates/universal-rules/project-routing.mdc) and [`adr-compliance.mdc`](../templates/universal-rules/adr-compliance.mdc) → `.cursor/rules/`.
5. Replace `REPLACE_WITH_PROJECT_NAME` in `index.md` and `project-routing.mdc`.
6. Customize the **Project structure** section in `project-routing.mdc` from the target README § Stack and verified repo layout (no invented paths).
7. When `adrs/` exists, ensure `adr-compliance.mdc` alignment-gaps path matches the project (`docs/adr-alignment-gaps.md` per universal ADR scaffold).
8. Copy [`rules-authoring.md`](../templates/universal-rules/rules-authoring.md) → `docs/rules-authoring.md` on large path; link from index § Maintenance.
9. Register each new rule in `index.md` (category, one-liner, activation) in the **same session** as the `.mdc` file.
10. Add or update `PROJ-RULE-*` rows in `docs/roadmap/backlog.md` with evidence paths.
11. When orchestration is scaffolded (`JR-ORCH-*`), copy [`workflow-gates.mdc`](../templates/universal-rules/workflow-gates.mdc) → `.cursor/rules/workflow-gates.mdc`; replace `REPLACE_WITH_*` placeholders; register in `index.md` (Workflow).

**Handoff rule:** Rules and the index must not link to the Jarvis repository. All `see` targets must be target-project paths.

## Layout conventions

```text
.cursor/
  rules/
    index.md                 # Markdown catalog (agents read explicitly)
    project-routing.mdc      # alwaysApply: true
    adr-compliance.mdc       # alwaysApply: true
    readme-governance.mdc    # globs: README.md (from universal-readme scaffold)
    <topic>.mdc              # globs or manual; one concern per file
  agents/                    # Optional; large path — contracts, not .mdc rules
    INDEX.md
```

| Convention | Default |
| --- | --- |
| Rule file format | `.mdc` with YAML frontmatter (`description`, `alwaysApply` and/or `globs`) |
| One concern per file | Prefer a new topic rule over growing `project-routing` |
| ADRs vs rules | ADRs record **decisions**; rules **enforce** or **route** — no long ADR paste in rules |
| Index authority | Every `.mdc` under `.cursor/rules/` has a row in `index.md` |
| Agents vs rules | Agent contracts live under `.cursor/agents/` (or `agents/`); index § Agent contracts points there — do not list agents as `.mdc` rules |

## Activation modes (summary)

| Mode | Frontmatter | Use for |
| --- | --- | --- |
| **alwaysApply** | `alwaysApply: true` | Routing, ADR compliance, repo-wide boundaries |
| **globs** | `globs: <pattern>` | Stack, APIs, tests, orchestration — load when touched |
| **manual** | Neither | Rare playbooks; list in index as **manual** |

**Full decision tree, budgets, and examples:** [`activation-modes.md`](./activation-modes.md) (`JR-RULE-001`). Default: **two** starter alwaysApply rules; pause before a **fourth**.

## Relationship to other Jarvis scaffolds

| Scaffold | Relationship |
| --- | --- |
| [`../universal-adr/README.md`](../universal-adr/README.md) | Rules cite **Accepted** ADRs; `adr-compliance.mdc` points at `adrs/INDEX.md` |
| [`../universal-readme/README.md`](../universal-readme/README.md) | `readme-governance.mdc` — globs `README.md`; index lists under Governance |
| [`../universal-docs/README.md`](../universal-docs/README.md) | **Doc-only** by default; optional `PROJ-RULE-*` doc-enforcement rule |
| [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) | README boundaries → `PROJ-RULE-*` and index rows |
| [`../universal-pr-commit/README.md`](../universal-pr-commit/README.md) | **Doc-only** PR/commit guide; index Workflow row — no default `.mdc` |
| [`../universal-validation/README.md`](../universal-validation/README.md) | Optional `validation-checklist.mdc` (globs); index Workflow row when checklist exists |
| [`../orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md) | Optional `workflow-gates.mdc` (globs) on **large** / orchestrated path — merge-ready **MG-*** contract (`JR-ORCH-004`) |
| [`../stack-scaffolding/selection.md`](../stack-scaffolding/selection.md) | After stack-profile — framework/library `.mdc` rules and `docs/stack/upstream-references.md` |
| [`../universal-agents/README.md`](../universal-agents/README.md) | Agent INDEX + role contract templates (`.cursor/agents/`) — `JR-AGENT-001`/`002` |
| [`activation-modes.md`](./activation-modes.md), [`authoring.md`](./authoring.md), [`adr-and-doc-citation.md`](./adr-and-doc-citation.md) | `JR-RULE-001`–`003` |

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- Replacing an existing `.cursor/rules/` tree that already encodes team-specific policy.
- Adding more than **three** `alwaysApply: true` rules without explicit approval (token and conflict risk).
- Converting a **doc-only** scaffold (for example documentation conventions) to always-apply without user direction.
- Deleting or renaming `.mdc` files referenced by **Accepted** ADRs, orchestration docs, or open `PROJ-RULE-*` tasks.
- Listing stack-specific rules in the index before commands or paths are **verified** from the repo.

Routine copy, placeholder replacement, index rows for new topic rules, and **Proposed** ADR drafts do not require extra approval.

## Decisions recorded for `JR-UNIVERSAL-004`

Defaults favor long-term agent efficiency; override per target when the user directs:

| Topic | Default |
| --- | --- |
| Rules directory | `.cursor/rules/` at repository root |
| Index file | `.cursor/rules/index.md` (Markdown, not `.mdc`) |
| Starter always-apply rules | `project-routing.mdc`, `adr-compliance.mdc` |
| Documentation conventions | **No** default `.mdc` — doc-only until [`JR-RULE-004`](../roadmap/backlog.md) (reference material pending) |
| README governance rule | From [`universal-readme`](../templates/universal-readme/readme-governance.mdc); globs `README.md`, not always-apply |
| PR/commit guide | **No** alwaysApply rule — [`pr-and-commit-guide.md`](../templates/universal-pr-commit/pr-and-commit-guide.md); Orchestrator applies when creating an orchestrated commit |
| Progressive enhancement / offline | **Not** scaffolded by Jarvis unless target declares support (README + ADRs) |
| Alignment gaps path | `docs/adr-alignment-gaps.md` (sync with universal ADR scaffold) |
| Agent contracts | Separate from rules index; optional `agents/` or `.cursor/agents/` on large path |
| WFD pattern source | Index categories and activation table generalized — no WFD product identifiers in templates |
