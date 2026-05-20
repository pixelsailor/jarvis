# Cursor rules authoring — REPLACE_WITH_PROJECT_NAME

How to add and maintain `.cursor/rules/*.mdc` files without duplicating ADRs or bloating every agent session.

**Canonical catalog:** [`.cursor/rules/index.md`](../.cursor/rules/index.md)

**Activation modes:** Prefer **globs** for stack and path-local boundaries; keep **alwaysApply** count low (platform default: two starters plus user-approved boundary rules). See Jarvis platform patterns at copy time (`activation-modes`, `authoring`, `adr-and-doc-citation` in universal-rules pack) — this file is the on-repo summary after handoff.

## When to create a rule vs a doc

| Situation | Prefer |
| --- | --- |
| Durable decision with alternatives and rationale | `adrs/ADR-NNN-*.md` |
| Long playbook, examples, or team policy narrative | `docs/<topic>.md` linked from README § Documentation |
| Short agent enforcement, routing, or “read X before Y” | `.cursor/rules/<topic>.mdc` |

Rules should **cite** ADRs and docs, not replace them.

## File shape

```yaml
---
description: One line for the rule picker and index “one-liner” column; include ADR-NNN when ADR-backed
alwaysApply: true          # OR omit and use globs (not both unless intentional)
globs: src/**/*.ts         # Optional; narrow patterns load fewer tokens
---
```

| Field | Guidance |
| --- | --- |
| `description` | States outcome (“Consult Accepted ADRs…”), not file names only |
| `alwaysApply` | Cross-cutting only; keep count low |
| `globs` | Prefer directory-scoped patterns over `**/*` unless truly global |

Recommended body header:

```markdown
## Scope (this rule)
**Owns:** …
**Does not own:** …
```

## One concern per file

Split when a rule exceeds roughly one screen of enforcement or mixes unrelated glob sets. Update `index.md` in the **same session**.

## Citing ADRs

- Reference **Accepted** ADRs by id (`ADR-003`) in `description` and body.
- Use **Binding** / **Supporting** labels with markdown links to `adrs/ADR-NNN-*.md`.
- Summarize the enforcement obligation in one or two sentences — do **not** paste ADR decision text.
- When an ADR is **Superseded**, update or remove rules that cite it in the same change set.
- Drift tracking: link [`docs/adr-alignment-gaps.md`](./adr-alignment-gaps.md); do not duplicate gap tables in topic rules.

## Citing docs and commands

- Link target-project paths only — never the Jarvis repository or sibling reference repos.
- Document development commands in README § Development or `docs/stack/commands.md`; rules may **name** scripts for enforcement, not invent them.
- Stack/vendor URLs: prefer [`docs/stack/upstream-references.md`](./stack/upstream-references.md) over long URL lists in every rule.

## Index maintenance

Every `.mdc` file needs a row in [`.cursor/rules/index.md`](../.cursor/rules/index.md): category, one-liner, activation mode. Remove rows when deleting rules. Add a `project-routing.mdc` row when the rule is a common entry point.

## Review triggers

Revisit rules when:

- An ADR moves to **Accepted** that changes boundaries the rule should enforce
- README § Boundaries or § Stack changes materially (sync `PROJ-RULE-*` / `PROJ-ADR-*` in `docs/roadmap/backlog.md` when the project uses that workflow)
- `alwaysApply` rules grow past three — consider glob-scoping or doc extraction
