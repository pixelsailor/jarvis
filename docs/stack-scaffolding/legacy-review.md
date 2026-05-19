# Legacy framework and library review (`JR-STACK-007`)

**Status:** Complete (2026-05-19)  
**Scope:** Jarvis repository trees that held stack-specific playbooks, rules, MCP configs, and library guides for copy into target projects.

## Decision (platform)

Jarvis **does not** maintain framework-, library-, or MCP-specific documentation or Cursor rules. Stack-specific agent guidance—including best practices, convention rules, MCP server setup, and tool order—belongs in the **target project** after initialization.

Jarvis stack scaffolding supplies **workflows and templates** only:

- Detect and confirm capabilities ([`detection.md`](./detection.md), [`confirmation.md`](./confirmation.md))
- Record commands, testing, runtime, and dependencies from the target repo
- Select **what to author** in the target using [`selection.md`](./selection.md) and [`upstream-capabilities.md`](./upstream-capabilities.md)
- Copy **generic** templates from [`../templates/stack-scaffolding/`](../templates/stack-scaffolding/) and [`../universal-*`](../universal-rules/README.md) families

**Rationale:** Framework docs drift with versions; duplicating them in Jarvis misleads agents and expands maintenance without improving handoff. Official upstream docs (`llms.txt`, vendor sites) plus target-owned `docs/stack/upstream-references.md` give agents a single, accurate index per project.

## Inventory and disposition

| Path | Role (legacy) | Disposition | Notes |
| --- | --- | --- | --- |
| `frameworks/*` | Framework playbooks (Svelte, React, Angular, Astro, WordPress) | **Removed** | Stale patterns (e.g. Svelte 3 `export let`); superseded by upstream |
| `libraries/*` | Library guides (Tailwind, Dexie, bits-ui) | **Removed** | Product-specific; target ADRs/rules own boundaries |
| `ai-agents/*` | Agent prompts, Angular rules, Svelte MCP order | **Removed** | MCP and agent workflows → target `upstream-references.md` / optional `.mdc` |
| `mcp-servers/*` | Example MCP JSON (Angular, Svelte) | **Removed** | Target `.cursor/mcp.json` or project docs |
| `.cursor/rules/*.mdc` (stack) | svelte-5, sveltekit-2, tailwind, angular | **Removed** | Target `.cursor/rules/<slug>-conventions.mdc` from templates |
| `mcp-tools/README.md` | Generic MCP integration essay | **Removed** | Out of Jarvis scope; target documents configured MCP |
| [`source-registry.md`](./source-registry.md) | Capability → Jarvis copy paths | **Replaced** by [`upstream-capabilities.md`](./upstream-capabilities.md) | Upstream URLs + target artifact names only |
| `.cursor.json` | Legacy instruction router | **Deprecated** | See [`JR-LEGACY-004`](../roadmap/backlog.md); points to platform docs |
| `jarvis-config.json` | “Extends Jarvis” template | **Deprecated** | See [`JR-LEGACY-003`](../roadmap/backlog.md) |
| `standards/` | Code style, naming, git | **Deferred** | [`JR-LEGACY-001`](../roadmap/backlog.md) — may generalize or remove |
| `architecture/`, `testing/`, `documentation/` | Generic-ish guides | **Deferred** | Heavy, uneven quality; not stack-scaffolding copy sources |
| `tools/` | Cursor/VS Code setup | **Deferred** | May fold into universal docs or remove |

## Generalized reuse (kept in Jarvis)

These patterns were extracted from legacy material into platform docs (no framework prose retained):

| Pattern | Where it lives |
| --- | --- |
| Composed capabilities (no profile IDs) | [`selection.md`](./selection.md), [`platform-spec.md`](../roadmap/platform-spec.md) |
| Upstream-first idioms; target-owned conventions | [`selection.md` § Official upstream](./selection.md#official-upstream-vs-jarvis-legacy) (renamed in place) |
| `upstream-references.md` + optional globs `.mdc` | [`../templates/stack-scaffolding/upstream-references.example.md`](../templates/stack-scaffolding/upstream-references.example.md), [`stack-framework-rule.example.mdc`](../templates/stack-scaffolding/stack-framework-rule.example.mdc) |
| MCP optional section when **target** configures tools | Same template § MCP |
| Strip Jarvis/WFD links on handoff | [`selection.md`](./selection.md), [`../universal-handoff/README.md`](../universal-handoff/README.md) |
| Normalized capability lookup | [`upstream-capabilities.md`](./upstream-capabilities.md) |

## Target project obligations (after init)

When Jarvis scaffolds stack-specific material, the **target** should own:

| Artifact | Purpose |
| --- | --- |
| `docs/stack/upstream-references.md` | Canonical external docs and MCP tool order |
| `.cursor/rules/<framework>-conventions.mdc` | Globs + project boundaries (from template, not Jarvis copy) |
| Optional `docs/stack/framework-guide.md` | **Target-only** conventions not in upstream docs |
| Optional `.cursor/mcp.json` | MCP servers the team actually uses |
| `PROJ-STACK-002` evidence | Paths created; registry slug and date |

## Agent efficiency

- **One lookup table** ([`upstream-capabilities.md`](./upstream-capabilities.md)) during init — no scanning removed Jarvis trees.
- **`upstream-references.md` in the target** is the runtime index for stack work; Jarvis docs are not opened for framework idioms after handoff.
- **Globs over alwaysApply** for stack rules (unchanged from `JR-STACK-002`).

## Human decisions (recorded)

| Question | Resolution |
| --- | --- |
| Keep Jarvis framework playbooks for “seed” content? | **No** — upstream + target templates only |
| Keep `ai-agents/` for MCP tool order? | **No** — document in target when MCP is configured |
| Maintain registry of Jarvis copy paths? | **No** — upstream-capabilities lists official docs and target artifact names |
| Preserve `standards/` in this task? | **Deferred** to `JR-LEGACY-001` (not framework trees) |

## Follow-up

- [`JR-LEGACY-001`](../roadmap/backlog.md) — inventory remaining legacy roots (`standards/`, `tools/`, etc.)
- [`JR-LEGACY-003`](../roadmap/backlog.md), [`JR-LEGACY-004`](../roadmap/backlog.md) — remove or replace deprecated config files
