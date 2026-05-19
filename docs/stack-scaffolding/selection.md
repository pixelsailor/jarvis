# Stack-specific rules and best-practices selection

After stack capabilities are confirmed in target `docs/stack/stack-profile.md` ([`confirmation.md`](./confirmation.md)), Jarvis selects **which** stack-specific Cursor rules, playbooks, and upstream references to copy or author in the target project.

**Platform task:** `JR-STACK-002`  
**Prerequisite:** `PROJ-STACK-000` complete (stack-profile recorded); universal rules scaffold present or planned ([`universal-rules`](../universal-rules/README.md)).  
**Related:** [`source-registry.md`](./source-registry.md) (capability → Jarvis copy sources); commands and scripts — [`commands.md`](./commands.md) (`JR-STACK-003`); testing prompts — `JR-STACK-004`.

## Decision: composed capabilities (not profile IDs)

Jarvis does **not** assign opaque stack profile IDs (for example `profile:sveltekit-pnpm`). Selection is **composed** from confirmed fields in `stack-profile.md`:

| Layer | Typical fields | Target output |
| --- | --- | --- |
| **Framework** | `framework` | One primary framework rule (`.mdc`) + optional `docs/stack/framework-guide.md` |
| **Language** | `primary_language` | Rows in `docs/stack/source-documentation.md` (from universal-docs template) |
| **Libraries** | `ui_library`, detected deps | Optional library topic rules or playbook sections — only when present in repo |
| **Tooling** | MCP / agent hints in registry | Optional `docs/stack/upstream-references.md` + narrow MCP rule when verified |

The [`source-registry.md`](./source-registry.md) maps **normalized capability values** to Jarvis legacy trees (`frameworks/`, `libraries/`, `ai-agents/`). Those trees are **copy sources**, not runtime dependencies. `JR-STACK-007` may retire or generalize registry rows.

## Agent read order (stack work)

1. Target `README.md` § Technology Stack  
2. `docs/stack/stack-profile.md` (including **Exclusions**)  
3. This document — run the [selection pass](#selection-pass)  
4. [`source-registry.md`](./source-registry.md) — resolve copy sources  
5. [`.cursor/rules/index.md`](../../templates/universal-rules/index.md) pattern — register new rules in the **same session**

## Selection pass

Run at the target **primary package path** from stack-profile.

### 1. Build the selection set

From **Confirmed capabilities** in stack-profile:

1. **Primary framework** — exactly one framework rule bundle unless user confirmed dual-stack (see [pause points](#human-input-pause-points)).
2. **Libraries** — add rows only when the capability appears in stack-profile **and** manifests (for example `tailwindcss` in `package.json`, `dexie` in dependencies).
3. **Upstream references** — always create or update `docs/stack/upstream-references.md` on **medium** and **large** init when production code exists; optional on **small** init.
4. **Skip** any registry row covered by **Exclusions** in stack-profile.

Do not select Jarvis sources for capabilities marked `unknown` or absent from stack-profile.

### 2. Resolve sources

For each selected capability, look up [`source-registry.md`](./source-registry.md):

| Registry status | Action |
| --- | --- |
| **Match** | Copy/adapt listed paths into target artifacts (below) |
| **Partial** | Use registry **notes**; prefer official upstream `llms.txt` / docs over long legacy playbooks |
| **No match** | Author a minimal target-only rule from README boundaries + verified repo layout; record gap on `PROJ-STACK-002` |

### 3. Adapt (required before commit)

When copying from Jarvis `frameworks/`, `libraries/`, or `ai-agents/`:

| Strip or rewrite | Why |
| --- | --- |
| Links to Jarvis, WFD, or other reference repos | Handoff self-containment |
| Invented `pnpm` / `npm` scripts not in target `package.json` | `JR-STACK-003` owns commands |
| Legacy syntax (Svelte 3 `export let`, Options API-only Angular guides) | Misleads agents unless target is legacy |
| Emoji-heavy tutorial prose | Noise for agents; keep checklists and routing |
| Product-specific identifiers from reference projects | Not portable |

**Prefer** official framework documentation (and `llms.txt` when listed in the registry) for **idioms**; use Jarvis legacy files for **routing tables** and **project-specific conventions** the user asked to preserve.

### 4. Write target artifacts

| Artifact | Path | Activation / role |
| --- | --- | --- |
| Framework topic rule | `.cursor/rules/<framework>-conventions.mdc` | `globs` for production paths (see [rule naming](#rule-naming-and-globs)) |
| Optional MCP / docs workflow rule | `.cursor/rules/<framework>-agent-docs.mdc` | **manual** or `globs` for `**/*.svelte` only when MCP is configured in target |
| Framework guide (condensed) | `docs/stack/framework-guide.md` | Linked from README § Documentation; WFD-scale playbooks **not** copied wholesale |
| Upstream references | `docs/stack/upstream-references.md` | Canonical URLs, `llms.txt`, MCP tool order — no secrets |
| Rules index rows | `.cursor/rules/index.md` | One row per new `.mdc`; activation mode documented |
| Routing row | `.cursor/rules/project-routing.mdc` | Add narrowest-match row pointing to stack rule |

Template for upstream file: [`upstream-references.example.md`](../templates/stack-scaffolding/upstream-references.example.md).  
Optional rule skeleton: [`stack-framework-rule.example.mdc`](../templates/stack-scaffolding/stack-framework-rule.example.mdc).

### 5. Cross-scaffold hooks

| Scaffold | Stack selection hook |
| --- | --- |
| [`universal-validation`](../universal-validation/README.md) | Fill checklist **stack-specific extensions** from framework + boundaries (same session as rules) |
| [`universal-docs`](../universal-docs/README.md) | `docs/stack/source-documentation.md` — language/framework comment syntax |
| [`universal-rules`](../universal-rules/README.md) | Keep **alwaysApply** count ≤ platform default; stack rules use **globs** unless user approves more |

### 6. Record backlog evidence

Close or update target `PROJ-STACK-002` with paths created, for example:

```markdown
- [x] `PROJ-STACK-002`: Add framework-specific rules and stack docs from confirmed capabilities.
  - Evidence: `.cursor/rules/svelte-conventions.mdc`, `docs/stack/upstream-references.md`, `docs/stack/framework-guide.md` (trimmed); registry: SvelteKit 2026-05-19.
```

## Rule naming and globs

| Convention | Default |
| --- | --- |
| Framework rule filename | `<slug>-conventions.mdc` where slug is lowercase registry slug (`svelte`, `react`, `angular`, …) |
| Globs | Match **verified** production roots from stack-profile and repo (e.g. `src/**/*.svelte`, `src/**/*.ts` for SvelteKit) |
| Second framework rule | **Do not** add without user confirmation of dual-stack |
| Library-specific rule | `<library>-conventions.mdc` only when library is in scope and conventions differ from framework rule |

**project-routing.mdc** stays a table of links — do not paste framework playbooks there.

## Initialization path defaults

| Path | Stack rules and docs |
| --- | --- |
| **Small** | Optional — `docs/stack/upstream-references.md` only, or one globs rule if agents will edit framework code |
| **Medium** | Default — one framework `.mdc` + `upstream-references.md`; add `framework-guide.md` when legacy Jarvis playbook adds non-obvious project conventions |
| **Large** | Required — medium set + library rules for each confirmed adjunct + optional MCP rule when target uses that MCP server |

Align with [`scaffolding-map.md`](../target-readme/scaffolding-map.md) step 5 (rules) after stack-profile exists.

## Official upstream vs Jarvis legacy

| Content type | Primary source | Fallback |
| --- | --- | --- |
| Framework idioms (runes, hooks, routing) | Official docs / `llms.txt` in upstream-references | Trimmed `frameworks/<name>/README.md` |
| MCP tool order (Svelte, etc.) | `ai-agents/<framework>/` when present | Author from vendor docs |
| UI library utilities (Tailwind, bits-ui) | Official docs + target config files | `libraries/<name>/README.md` (heavy — summarize) |
| Product boundaries | Target README + ADRs | Never from Jarvis legacy |

## Human input (pause points)

Jarvis must **stop and ask** before:

| Situation | Question intent |
| --- | --- |
| Two frameworks equally primary at the same package root | Which framework gets agent rules; other → Exclusions or path-scoped globs |
| Target already has `.cursor/rules/*-conventions.mdc` | Merge vs replace vs skip |
| Registry **no match** for confirmed framework | Minimal custom rule only, or defer stack rules until human supplies a source doc |
| User wants **doc-only** (no stack `.mdc`) | Confirm — upstream-references + README map only |
| Copy would add a **third** `alwaysApply: true` stack rule | User approval ([`universal-rules`](../universal-rules/README.md)) |
| Legacy Jarvis playbook contradicts detected major version (Svelte 3 vs 5, Angular version) | Which version agents should follow |
| Optional: MCP / IDE doc workflow | Whether to add MCP rule (e.g. Svelte MCP) when tools are configured in target environment |

Routine selection from a confirmed stack-profile, one framework globs rule, and upstream-references does **not** require extra approval.

## Agent efficiency notes

- **One selection pass** per init after `stack-profile.md` — do not re-scan Jarvis `frameworks/` on every task; record choices in `PROJ-STACK-002` evidence.
- **upstream-references.md** is the lightweight index agents open first; **framework-guide.md** holds only target-specific conventions not covered upstream.
- **Globs over alwaysApply** for stack rules keeps cold sessions smaller.
- Re-run selection when stack-profile [re-verification triggers](./confirmation.md#re-verification-triggers) fire; update index and routing in the same session.

## WFD and reference repos

What's For Dinner informs **discipline** (orchestration, ADR usage), not automatic stack rule content. Do not copy WFD `.cursor/rules/` into unrelated targets. Detection and selection use the **target path only**.
