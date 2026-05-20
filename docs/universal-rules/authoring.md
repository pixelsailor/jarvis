# Rule authoring for target projects (`JR-RULE-002`)

How Jarvis and target-project maintainers create `.cursor/rules/*.mdc` files that stay short, discoverable, and independent after handoff.

**Platform task:** `JR-RULE-002`  
**Target paste:** [`../templates/universal-rules/rules-authoring.md`](../templates/universal-rules/rules-authoring.md) → `docs/rules-authoring.md` on **large** init  
**Related:** [`activation-modes.md`](./activation-modes.md) (`JR-RULE-001`), [`adr-and-doc-citation.md`](./adr-and-doc-citation.md) (`JR-RULE-003`), [`README.md`](./README.md) (scaffold copy steps)

## Rule vs ADR vs doc

| Artifact | Records | Agent load |
| --- | --- | --- |
| **ADR** (`adrs/ADR-NNN-*.md`) | Decision, alternatives, status, consequences | Read when architectural; cited by rules |
| **Doc** (`docs/<topic>.md`) | Playbooks, examples, maintenance policy | README § Documentation map; rules link |
| **Rule** (`.cursor/rules/<topic>.mdc`) | Enforcement, routing, “before you edit X read Y” | Frontmatter activation + index row |

**Split test:** If the text is mostly narrative or will change with stack upgrades, it belongs in a **doc**. If deleting the file would let agents violate a boundary silently, it belongs in a **rule** (or an ADR plus a one-paragraph rule pointer).

## File and naming conventions

| Item | Convention |
| --- | --- |
| Location | `.cursor/rules/` at repository root |
| Extension | `.mdc` with YAML frontmatter |
| Filename | `kebab-case.mdc`; stack: `<slug>-conventions.mdc` per [`selection.md`](../stack-scaffolding/selection.md) |
| One concern | One primary topic per file; split when glob sets or ADR domains differ |
| Index | [`index.md`](../templates/universal-rules/index.md) — Markdown catalog, not a rule file |

## Frontmatter shape

```yaml
---
description: Outcome-oriented one line; include primary ADR id when ADR-backed
alwaysApply: true          # XOR default: use globs instead
globs: src/**/*.ts         # Verified paths only
---
```

| Field | Guidance |
| --- | --- |
| `description` | What the agent must do or avoid — not “see adr-compliance.mdc” alone |
| `alwaysApply` | Cross-cutting only; see [`activation-modes.md`](./activation-modes.md) |
| `globs` | Narrowest verified patterns; document in index one-liner |

Body structure (recommended):

```markdown
# <Topic> — <ProjectName>

## Scope (this rule)
**Owns:** …
**Does not own:** …

## …
```

The **Scope** block prevents overlap with `project-routing` and stack rules.

## Authoring workflow (same session)

1. Choose mode — [`activation-modes.md`](./activation-modes.md) decision tree.
2. Draft `.mdc` — Scope + enforcement bullets + links ([`adr-and-doc-citation.md`](./adr-and-doc-citation.md)).
3. Add or update row in `.cursor/rules/index.md` (category, one-liner, activation).
4. Add routing row in `project-routing.mdc` when the rule is a common entry point.
5. Update `docs/roadmap/backlog.md` `PROJ-RULE-*` with evidence paths when the project uses setup backlog.
6. If README § Boundaries changed, sync per [`../target-roadmap/readme-sync.md`](../target-roadmap/readme-sync.md).

Never commit a new `.mdc` without an index row.

## Content limits (readability and tokens)

| Limit | Guidance |
| --- | --- |
| Length | Prefer one screen of enforcement; move examples to `docs/` |
| Tables | Routing tables OK; do not paste ADR decision tables |
| Commands | Repeat only names needed for enforcement; canonical list in README § Development or `docs/stack/commands.md` |
| Links | Target-repo paths only — never Jarvis, WFD, or sibling repos |

## Categories (index columns)

Use consistent index categories so agents scan quickly:

| Category | Examples |
| --- | --- |
| **Routing** | `project-routing` |
| **Governance** | `adr-compliance`, `readme-governance` |
| **ADR-backed** | Boundary rules citing `ADR-NNN` |
| **Stack** | Framework/library conventions |
| **Workflow** | `workflow-gates`, `validation-checklist`, PR/commit (doc or rule) |

## Review triggers

Revisit rules when:

- An ADR moves to **Accepted** and changes enforceable boundaries
- README § Boundaries, § Stack, or § Documentation map changes materially
- `alwaysApply` count approaches three — demote or extract docs
- Glob targets move in refactor — update patterns and index in one change set
- Orchestration scaffold added — register `workflow-gates.mdc` and orchestration globs rules

## Progressive enhancement and offline-style rules

Jarvis **does not** add offline-first, connectivity-capability, or similar **progressive-enhancement** rules unless the target README and **Accepted** ADRs explicitly require that product model. When the target declares support, author ADRs first, then globs or user-approved alwaysApply rules — never copy from reference projects (e.g. WFD) by default.

## Human input (pause points)

Jarvis must **stop and ask** before:

- Scaffolding offline/progressive-enhancement rules without README/ADR evidence
- Replacing an existing `.cursor/rules/` tree that encodes team policy
- Deleting or renaming `.mdc` files referenced by **Accepted** ADRs or open `PROJ-RULE-*`
- Splitting a rule the user asked to keep unified
- Authoring rules before stack paths are verified (invented globs)

Routine authoring from templates, index rows, and **Proposed** ADRs does not require extra approval.

## Decisions recorded for `JR-RULE-002`

| Topic | Default |
| --- | --- |
| Target authoring doc | `docs/rules-authoring.md` on **large** path only; medium relies on index + platform patterns at copy time |
| Required body sections | Scope (owns / does not own) for non-trivial rules |
| Index update | Same session as any `.mdc` create/change/delete |
| WFD-style dense alwaysApply stacks | Not copied — composed per target boundaries |
