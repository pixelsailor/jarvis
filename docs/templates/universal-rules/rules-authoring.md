# Cursor rules authoring — REPLACE_WITH_PROJECT_NAME

How to add and maintain `.cursor/rules/*.mdc` files without duplicating ADRs or bloating every agent session.

**Canonical catalog:** [`.cursor/rules/index.md`](../.cursor/rules/index.md)

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
description: One line for the rule picker and index “one-liner” column
alwaysApply: true          # OR omit and use globs (not both unless intentional)
globs: src/**/*.ts         # Optional; narrow patterns load fewer tokens
---
```

| Field | Guidance |
| --- | --- |
| `description` | States outcome (“Consult Accepted ADRs…”), not file names only |
| `alwaysApply` | Cross-cutting only; keep count low |
| `globs` | Prefer directory-scoped patterns over `**/*` unless truly global |

## One concern per file

Split when a rule exceeds roughly one screen of enforcement or mixes unrelated glob sets. Update `index.md` in the **same session**.

## Citing ADRs

- Reference **Accepted** ADRs by id in the rule body (`ADR-003`) and link the file.
- Do **not** paste the ADR decision text — summarize the enforcement obligation in one or two sentences.
- When an ADR is **Superseded**, update or remove rules that cite it in the same change set.

## Citing docs and commands

- Link target-project paths only — never the Jarvis repository.
- Document development commands in README § Development or a verified stack doc; rules may **repeat** only the command names needed for enforcement, not invent scripts.

## Index maintenance

Every `.mdc` file needs a row in [`.cursor/rules/index.md`](../.cursor/rules/index.md): category, one-liner, activation mode. Remove rows when deleting rules.

## Review triggers

Revisit rules when:

- An ADR moves to **Accepted** that changes boundaries the rule should enforce
- README § Boundaries or § Stack changes materially (sync `PROJ-RULE-*` / `PROJ-ADR-*` in `docs/roadmap/backlog.md` when the project uses that workflow)
- `alwaysApply` rules grow past three — consider glob-scoping or doc extraction
