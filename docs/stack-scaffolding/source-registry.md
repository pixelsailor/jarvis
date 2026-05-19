# Stack source registry (Jarvis → target)

Maps **normalized capability values** from target `docs/stack/stack-profile.md` to Jarvis repository paths used during [selection](./selection.md). This is a **lookup table**, not a stack profile catalog.

**Maintenance:** Add rows when new `frameworks/` or `libraries/` trees are reviewed (`JR-STACK-007`). Remove or mark **legacy** rows when upstream docs supersede them.

## Normalization

Before lookup, normalize stack-profile values:

| Rule | Example |
| --- | --- |
| Lowercase | `SvelteKit` → `sveltekit` |
| Strip version suffixes in prose | `Angular 20` → `angular` |
| Map common aliases | `Next` / `Next.js` → `nextjs`; `Vue 3` → `vue` |
| Monorepo scope | Apply registry at **primary package path** only |

If multiple aliases match, prefer the **most specific** row (for example `sveltekit` over `svelte`).

## Framework registry

| Normalized `framework` | Jarvis sources (copy/adapt) | Upstream (prefer for idioms) | Target artifacts (default) |
| --- | --- | --- | --- |
| `sveltekit` | `frameworks/svelte/README.md`; `ai-agents/svelte/agents.md` | [Svelte](https://svelte.dev/docs/svelte/llms.txt), [SvelteKit](https://svelte.dev/docs/kit/llms.txt), [best practices](https://svelte.dev/docs/svelte/best-practices/llms.txt) | `svelte-conventions.mdc`, `upstream-references.md`; optional `framework-guide.md` |
| `svelte` | `frameworks/svelte/README.md` | Same Svelte llms.txt links | Same; confirm not SvelteKit before using kit-only globs |
| `react` | `frameworks/react/README.md` | [React](https://react.dev) | `react-conventions.mdc` |
| `nextjs` | `frameworks/react/README.md` *(partial)* | [Next.js docs](https://nextjs.org/docs) | `nextjs-conventions.mdc`; author Next-specific globs |
| `angular` | `frameworks/angular/README.md`; `ai-agents/angular/llms.txt`, `ai-agents/angular/rules/cursor/angular-20.mdc` | [Angular docs](https://angular.dev) | `angular-conventions.mdc` |
| `astro` | `frameworks/astro/README.md` | [Astro docs](https://docs.astro.build) | `astro-conventions.mdc` |
| `vue` / `nuxt` | — *(no Jarvis tree yet)* | [Vue](https://vuejs.org), [Nuxt](https://nuxt.com/docs) | Author minimal rule from repo layout |
| `django` / `fastapi` / `flask` | — | Official Python framework docs | Language globs + `source-documentation.md` |
| `rails` | — | [Rails guides](https://guides.rubyonrails.org) | Conventions rule with `app/`, `config/` globs |
| `wordpress` | `frameworks/wordpress/README.md` | [WordPress Developer Resources](https://developer.wordpress.org) | Confirm product vs theme-only ([`detection.md`](./detection.md)) |

**Partial** = Jarvis file is incomplete for that framework; do not copy blindly.

## Library registry (adjunct)

Select only when the library appears in stack-profile **and** target manifests.

| Normalized value | Jarvis sources | Upstream | Typical target artifact |
| --- | --- | --- | --- |
| `tailwindcss` / `tailwind` | `libraries/tailwindcss/README.md` | [Tailwind docs](https://tailwindcss.com/docs) | Section in `framework-guide.md` or `tailwind-conventions.mdc` with `**/*.{svelte,tsx,vue}` globs |
| `dexie` | `libraries/dexie/README.md` | [Dexie.js](https://dexie.org) | Topic rule or DATA-/ADR-driven doc — not universal |
| `bits-ui` | `libraries/bits-ui/README.md` | Project / shadcn-svelte docs as applicable | Globs on component paths only |

## Language → documentation

| `primary_language` | Jarvis source | Target artifact |
| --- | --- | --- |
| `typescript` / `javascript` | — (use universal-docs template) | `docs/stack/source-documentation.md` § TS/JS |
| `python` | — | § Python in `source-documentation.md` |
| Other | — | Add section from official style guide; no `frameworks/` row |

## ai-agents/ usage

| Path | Use in target |
| --- | --- |
| `ai-agents/svelte/agents.md` | MCP tool order → `upstream-references.md` or `svelte-agent-docs.mdc` (**manual**) |
| `ai-agents/angular/llms.txt` | Link list in `upstream-references.md` |
| `ai-agents/angular/rules/cursor/*.mdc` | Adapt to target Angular version; register in index |
| `ai-agents/README.md` | **Do not** copy wholesale — generic agent patterns only |

## No match workflow

When the confirmed framework has **no row**:

1. Record `registry: no match` on `PROJ-STACK-002`.
2. Create `docs/stack/upstream-references.md` with official docs only.
3. Add a minimal `.cursor/rules/<slug>-conventions.mdc` from README § Boundaries and verified directories.
4. Optionally ask the user for an internal playbook path to adapt.

## Version and legacy warnings

| Signal | Registry behavior |
| --- | --- |
| Svelte 5 runes in target `package.json` | Prefer Svelte llms.txt; strip Svelte 3 patterns from `frameworks/svelte/` |
| `@angular/core` major ≠ Jarvis `angular-20.mdc` | Adapt or skip Jarvis mdc; cite angular.dev |
| Create React App vs Vite | Globs and guides differ — detect via `vite.config.*` ([`detection.md`](./detection.md)) |
