# Stack-specific rules and best-practices selection

After stack capabilities are confirmed in target `docs/stack/stack-profile.md` ([`confirmation.md`](./confirmation.md)), Jarvis selects **which** stack-specific Cursor rules and docs to **author in the target project**—and records upstream links agents should use.

**Platform task:** `JR-STACK-002`  
**Prerequisite:** `PROJ-STACK-000` complete (stack-profile recorded); universal rules scaffold present or planned ([`universal-rules`](../universal-rules/README.md)).  
**Related:** [`upstream-capabilities.md`](./upstream-capabilities.md) (capability → upstream URLs and target artifacts); commands — [`commands.md`](./commands.md) (`JR-STACK-003`); testing — [`testing.md`](./testing.md) (`JR-STACK-004`); deps — [`dependencies.md`](./dependencies.md) (`JR-STACK-006`). Legacy copy sources removed: [`legacy-review.md`](./legacy-review.md) (`JR-STACK-007`).

## Decision: composed capabilities (not profile IDs)

Jarvis does **not** assign opaque stack profile IDs (for example `profile:sveltekit-pnpm`). Selection is **composed** from confirmed fields in `stack-profile.md`:

| Layer | Typical fields | Target output |
| --- | --- | --- |
| **Framework** | `framework` | One primary framework rule (`.mdc`) + optional `docs/stack/framework-guide.md` |
| **Language** | `primary_language` | Rows in `docs/stack/source-documentation.md` (from universal-docs template) |
| **Libraries** | `ui_library`, detected deps | Optional library topic rules or guide sections — only when present in repo |
| **Tooling** | MCP when user confirms | § in `upstream-references.md` + optional narrow MCP rule in **target** |

Jarvis does **not** host framework playbooks or MCP workflows. [`upstream-capabilities.md`](./upstream-capabilities.md) lists official docs and default target paths only.

## Agent read order (stack work)

1. Target `README.md` § Technology Stack  
2. `docs/stack/stack-profile.md` (including **Exclusions**)  
3. This document — run the [selection pass](#selection-pass)  
4. [`upstream-capabilities.md`](./upstream-capabilities.md) — resolve upstream links and artifact names  
5. [`.cursor/rules/index.md`](../../templates/universal-rules/index.md) pattern — register new rules in the **same session**

## Selection pass

Run at the target **primary package path** from stack-profile.

### 1. Build the selection set

From **Confirmed capabilities** in stack-profile:

1. **Primary framework** — exactly one framework rule bundle unless user confirmed dual-stack (see [pause points](#human-input-pause-points)).
2. **Libraries** — add only when the capability appears in stack-profile **and** manifests (for example `tailwindcss` in `package.json`).
3. **Upstream references** — create or update `docs/stack/upstream-references.md` on **medium** and **large** init when production code exists; optional on **small** init.
4. **Skip** capabilities listed under **Exclusions** in stack-profile.

Do not author rules for capabilities marked `unknown` or absent from stack-profile.

### 2. Resolve upstream and artifacts

For each selected capability, look up [`upstream-capabilities.md`](./upstream-capabilities.md):

| Lookup result | Action |
| --- | --- |
| **Match** | Author listed target artifacts; populate `upstream-references.md` from upstream column |
| **No match** | Author minimal rule from README boundaries + verified repo layout; record `registry: no match` on `PROJ-STACK-002` |

### 3. Author from templates (required before commit)

Use [`../templates/stack-scaffolding/`](../templates/stack-scaffolding/) and target README/ADRs—not Jarvis framework trees.

| Rule | Why |
| --- | --- |
| No links to Jarvis, WFD, or other reference repos | Handoff self-containment |
| No invented scripts | [`commands.md`](./commands.md) owns commands from target manifests |
| Idioms from official docs / `llms.txt` | Version-accurate; see upstream-capabilities |
| `framework-guide.md` only for **target-specific** conventions | Keeps agents on upstream for syntax |
| Product boundaries from target README + ADRs | Not portable from other projects |

### 4. Write target artifacts

| Artifact | Path | Activation / role |
| --- | --- | --- |
| Framework topic rule | `.cursor/rules/<framework>-conventions.mdc` | `globs` for production paths (see [rule naming](#rule-naming-and-globs)) |
| Optional MCP / docs workflow rule | `.cursor/rules/<framework>-agent-docs.mdc` | **manual** when user confirmed MCP in target |
| Framework guide (condensed) | `docs/stack/framework-guide.md` | Target-only conventions; link from README § Documentation |
| Upstream references | `docs/stack/upstream-references.md` | Canonical URLs, `llms.txt`, MCP tool order — no secrets |
| Rules index rows | `.cursor/rules/index.md` | One row per new `.mdc`; activation mode documented |
| Routing row | `.cursor/rules/project-routing.mdc` | Narrowest-match row pointing to stack rule |

Templates: [`upstream-references.example.md`](../templates/stack-scaffolding/upstream-references.example.md), [`stack-framework-rule.example.mdc`](../templates/stack-scaffolding/stack-framework-rule.example.mdc).

### 5. Cross-scaffold hooks

| Scaffold | Stack selection hook |
| --- | --- |
| [`universal-validation`](../universal-validation/README.md) | Fill checklist **stack-specific extensions** from framework + boundaries (same session as rules) |
| [`universal-docs`](../universal-docs/README.md) | `docs/stack/source-documentation.md` — language/framework comment syntax |
| [`universal-rules`](../universal-rules/README.md) | Keep **alwaysApply** count ≤ platform default; stack rules use **globs** unless user approves more — [`activation-modes.md`](../universal-rules/activation-modes.md), [`adr-and-doc-citation.md`](../universal-rules/adr-and-doc-citation.md) |

### 6. Record backlog evidence

Close or update target `PROJ-STACK-002` with paths created, for example:

```markdown
- [x] `PROJ-STACK-002`: Add framework-specific rules and stack docs from confirmed capabilities.
  - Evidence: `.cursor/rules/svelte-conventions.mdc`, `docs/stack/upstream-references.md`; upstream: SvelteKit 2026-05-19.
```

## Rule naming and globs

| Convention | Default |
| --- | --- |
| Framework rule filename | `<slug>-conventions.mdc` where slug is lowercase (`svelte`, `react`, `angular`, …) |
| Globs | Match **verified** production roots from stack-profile and repo |
| Second framework rule | **Do not** add without user confirmation of dual-stack |
| Library-specific rule | `<library>-conventions.mdc` only when library is in scope and conventions differ from framework rule |

**project-routing.mdc** stays a table of links — do not paste framework playbooks there.

## Initialization path defaults

| Path | Stack rules and docs |
| --- | --- |
| **Small** | Optional — `docs/stack/upstream-references.md` only, or one globs rule if agents will edit framework code |
| **Medium** | Default — one framework `.mdc` + `upstream-references.md`; add `framework-guide.md` when target has non-obvious conventions |
| **Large** | Required — medium set + library rules for each confirmed adjunct + optional MCP rule when target configures MCP |

Align with [`scaffolding-map.md`](../target-readme/scaffolding-map.md) step 5 (rules) after stack-profile exists.

## Content sources

| Content type | Source |
| --- | --- |
| Framework idioms (runes, hooks, routing) | Official docs / `llms.txt` in target `upstream-references.md` |
| MCP tool order | Target `upstream-references.md` + vendor docs; optional target `.mdc` |
| UI libraries (Tailwind, etc.) | Official docs + target config files; short target rule or guide section |
| Product boundaries | Target README + ADRs |

## Human input (pause points)

Jarvis must **stop and ask** before:

| Situation | Question intent |
| --- | --- |
| Two frameworks equally primary at the same package root | Which framework gets agent rules; other → Exclusions or path-scoped globs |
| Target already has `.cursor/rules/*-conventions.mdc` | Merge vs replace vs skip |
| **No match** in upstream-capabilities | Minimal custom rule only, or defer until human supplies a source doc |
| User wants **doc-only** (no stack `.mdc`) | Confirm — upstream-references + README map only |
| Adding a **third** `alwaysApply: true` stack rule | User approval ([`universal-rules`](../universal-rules/README.md)) |
| Detected major version ambiguous | Which version agents should follow (document in upstream-references) |
| MCP / IDE doc workflow | Whether to document MCP in target when tools are configured |

Routine selection from a confirmed stack-profile, one framework globs rule, and upstream-references does **not** require extra approval.

## Agent efficiency notes

- **One selection pass** per init after `stack-profile.md` — record choices in `PROJ-STACK-002` evidence; do not re-resolve on every task.
- **`upstream-references.md` in the target** is the runtime index for stack work after handoff.
- **`framework-guide.md`** holds only target-specific conventions not covered upstream.
- **Globs over alwaysApply** for stack rules keeps cold sessions smaller.
- Re-run selection when stack-profile [re-verification triggers](./confirmation.md#re-verification-triggers) fire; update index and routing in the same session.

## WFD and reference repos

What's For Dinner informs **discipline** (orchestration, ADR usage), not automatic stack rule content. Do not copy WFD `.cursor/rules/` into unrelated targets. Detection and selection use the **target path only**.
