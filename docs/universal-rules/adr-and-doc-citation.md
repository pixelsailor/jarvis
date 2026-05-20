# ADR and doc citation in rules (`JR-RULE-003`)

How generated and maintained `.cursor/rules/*.mdc` files reference **Accepted** ADRs and target-project docs without duplicating decisions or breaking handoff.

**Platform task:** `JR-RULE-003`  
**Related:** [`authoring.md`](./authoring.md) (`JR-RULE-002`), [`../universal-adr/README.md`](../universal-adr/README.md), [`../templates/universal-rules/adr-compliance.mdc`](../templates/universal-rules/adr-compliance.mdc)

## Separation of concerns

| Layer | Holds | Rule cites it by |
| --- | --- | --- |
| **ADR** | Decision, status, alternatives | `ADR-NNN` id + markdown link to `adrs/ADR-NNN-*.md` |
| **Alignment gaps** | Implementation drift | Link `docs/adr-alignment-gaps.md` from `adr-compliance` — not duplicated per topic rule |
| **Topic doc** | Playbooks, syntax, checklists | Relative path `docs/...` from rule body |
| **README** | 10k-ft intent, doc map | Link sections; rules do not replace README |
| **Stack profile / commands** | Verified tooling | `docs/stack/commands.md`, `stack-profile.md` — commands not invented in rules |

Rules **enforce** and **route**. They do not archive architecture history.

## ADR citation format

### In `description` (frontmatter)

Use when the rule exists primarily to enforce one or more ADRs:

```yaml
description: Enforce ADR-004 and ADR-006 cloud boundary; local continuity over sync
```

| Pattern | When |
| --- | --- |
| `ADR-NNN` only | Single binding ADR |
| `ADR-NNN` and `ADR-MMM` | Multiple Accepted ADRs of equal weight |
| No ADR id | Routing-only or stack-upstream rules (framework conventions) |

Avoid filenames alone in `description` — ids are stable across renames if the index is updated.

### In rule body

```markdown
**Binding:** [ADR-004: Account and cloud enhancement](../adrs/ADR-004-account-and-cloud-enhancement.md) — enhancement optional; local recipe authority unchanged.

**Supporting:** [ADR-002: Local data ownership](../adrs/ADR-002-local-data-ownership.md) — Dexie as system of record.
```

| Tag | Use |
| --- | --- |
| **Binding** | Accepted ADRs the rule directly enforces |
| **Supporting** | Accepted ADRs that provide context; violation still possible without reading these first |
| **Related** | Docs or rules — cross-navigation only |

**Enforcement sentence:** After citations, one or two sentences state what the agent **must** do — paraphrase, do not paste the ADR “Decision” section.

### ADR status handling

| Status | Rule behavior |
| --- | --- |
| **Accepted** | Cite as binding or supporting; enforce in body |
| **Proposed** | Do not cite as binding; rule may say “draft ADR pending” only if user directed |
| **Deprecated / Superseded** | Remove or update citations in the **same change set** as index/ADR update |
| **No `adrs/` yet** | `adr-compliance.mdc` points agents to README § Boundaries; cite **Proposed** after draft |

### Gap identifiers

When the project tracks drift (e.g. `GAP-001`):

- Mention gap ids in rules **only** when a rule’s enforcement depends on known drift (Validator, offline UX).
- Canonical list stays in `docs/adr-alignment-gaps.md` — rules link there; do not fork gap tables.

## Doc citation format

### Paths

| Rule | Example |
| --- | --- |
| Relative from `.cursor/rules/` | [`docs/stack/commands.md`](../../docs/stack/commands.md) |
| Same for `adrs/` | [`adrs/INDEX.md`](../../adrs/INDEX.md) |
| No absolute URLs to Jarvis/WFD | Handoff **IND** checks fail |

### README § Documentation map

When README lists `docs/validation-checklist.md`:

- Workflow rules cite that path.
- Add index **Workflow** row when the file exists.
- Open `PROJ-DOC-*` until file exists — do not cite missing paths as if present.

### Commands and scripts

| Source of truth | Rule may |
| --- | --- |
| `package.json` scripts, `docs/stack/commands.md` | Name scripts agents must run (e.g. `pnpm run check`) |
| Invented commands | **Never** |

Repeat at most the command names needed for enforcement; full tables live in docs.

### Upstream / vendor docs

Stack rules cite **target** `docs/stack/upstream-references.md` first. Vendor URLs live there — not repeated at length in every `.mdc`.

## Anti-patterns

| Anti-pattern | Fix |
| --- | --- |
| Pasting ADR “Context” or “Decision” into a rule | Link + short enforcement obligation |
| Citing **Proposed** as binding | Wait for Accepted or enforce via README only |
| `see adr-compliance` with no ADR ids on an ADR-backed boundary rule | Add **Binding** ADR ids |
| Linking `../../jarvis/...` or WFD paths | Target-only paths |
| One rule citing ten ADRs | Split by domain or glob scope |
| Duplicating `docs/documentation-conventions.md` in alwaysApply | Keep conventions in doc; optional globs rule |

## Citation checklist (per new ADR-backed rule)

- [ ] `description` includes primary `ADR-NNN` when ADR-backed
- [ ] Body lists **Binding** / **Supporting** with markdown links to `adrs/`
- [ ] Enforcement paragraph is ≤ ~3 sentences
- [ ] `index.md` row one-liner mentions ADR id(s)
- [ ] `project-routing.mdc` has a narrowest-match row if agents often need this rule
- [ ] No Jarvis or reference-repo links
- [ ] Globs match verified paths when boundary is path-local

## Jarvis copy/adapt steps

When scaffolding from [`adr-compliance.mdc`](../templates/universal-rules/adr-compliance.mdc) and boundary rules from README § Boundaries:

1. Map each non-negotiable to **Accepted** ADR (or `PROJ-ADR-*` to create one).
2. Author one globs or alwaysApply rule per enforceable boundary cluster — [`activation-modes.md`](./activation-modes.md).
3. Apply citation format above; register in index.
4. Point `adr-compliance.mdc` at the project’s gaps file path if not `docs/adr-alignment-gaps.md`.

## Human input (pause points)

Jarvis must **stop and ask** before:

- Citing **Proposed** ADRs as binding in rules
- Adding gap ids to alwaysApply rules without user confirmation (noise in every session)
- Changing the project’s alignment-gaps path without updating `adr-compliance` and index
- Rules that contradict an **Accepted** ADR (requires ADR amendment or explicit user override)

## Decisions recorded for `JR-RULE-003`

| Topic | Default |
| --- | --- |
| Binding vs Supporting labels | Use in ADR-backed rule bodies |
| Frontmatter | Primary ADR id(s) in `description` when ADR-backed |
| Gaps | Single doc; rules link, no per-gap rule sprawl |
| Alignment gaps path | `docs/adr-alignment-gaps.md` unless user chooses another (sync `adr-compliance`) |
| Vendor docs | Indirection via `docs/stack/upstream-references.md` |
