# Upstream capabilities index (target authoring)

Maps **normalized capability values** from target `docs/stack/stack-profile.md` to **official documentation** and **default target artifacts** Jarvis should create during initialization. Jarvis does **not** host framework or library playbooks; agents use this table during [`selection.md`](./selection.md) only.

**Platform task:** `JR-STACK-002` (with [`selection.md`](./selection.md))  
**Review:** Legacy copy sources removed per [`legacy-review.md`](./legacy-review.md) (`JR-STACK-007`).

## Normalization

Before lookup, normalize stack-profile values:

| Rule | Example |
| --- | --- |
| Lowercase | `SvelteKit` → `sveltekit` |
| Strip version suffixes in prose | `Angular 20` → `angular` |
| Map common aliases | `Next` / `Next.js` → `nextjs`; `Vue 3` → `vue` |
| Monorepo scope | Apply at **primary package path** only |

If multiple aliases match, prefer the **most specific** row (for example `sveltekit` over `svelte`).

## Framework index

| Normalized `framework` | Upstream (prefer for idioms) | Target artifacts (default) |
| --- | --- | --- |
| `sveltekit` | [Svelte](https://svelte.dev/docs/svelte/llms.txt), [SvelteKit](https://svelte.dev/docs/kit/llms.txt), [best practices](https://svelte.dev/docs/svelte/best-practices/llms.txt) | `svelte-conventions.mdc`, `upstream-references.md`; optional `framework-guide.md` |
| `svelte` | Same Svelte links | Same; confirm not SvelteKit before kit-only globs |
| `react` | [React](https://react.dev) | `react-conventions.mdc`, `upstream-references.md` |
| `nextjs` | [Next.js docs](https://nextjs.org/docs) | `nextjs-conventions.mdc`, `upstream-references.md` |
| `angular` | [Angular docs](https://angular.dev) | `angular-conventions.mdc`, `upstream-references.md` |
| `astro` | [Astro docs](https://docs.astro.build) | `astro-conventions.mdc`, `upstream-references.md` |
| `vue` / `nuxt` | [Vue](https://vuejs.org), [Nuxt](https://nuxt.com/docs) | Minimal rule from repo layout + upstream links |
| `django` / `fastapi` / `flask` | Official Python framework docs | Language section in `source-documentation.md` + conventions rule with verified paths |
| `rails` | [Rails guides](https://guides.rubyonrails.org) | Conventions rule with `app/`, `config/` globs |
| `wordpress` | [WordPress Developer Resources](https://developer.wordpress.org) | Confirm product vs theme-only ([`detection.md`](./detection.md)) |

When **no row** matches: record `registry: no match` on `PROJ-STACK-002`; create `upstream-references.md` from official docs only; author minimal `.cursor/rules/<slug>-conventions.mdc` from README boundaries.

## Library index (adjunct)

Select only when the library appears in stack-profile **and** target manifests.

| Normalized value | Upstream | Typical target artifact |
| --- | --- | --- |
| `tailwindcss` / `tailwind` | [Tailwind docs](https://tailwindcss.com/docs) | Section in `framework-guide.md` or `tailwind-conventions.mdc` with verified globs |
| `dexie` | [Dexie.js](https://dexie.org) | Topic rule or DATA-/ADR-driven doc — project-specific |
| `bits-ui` | Upstream package / design-system docs for the version in use | Globs on component paths only |

## Language → documentation

| `primary_language` | Upstream / template | Target artifact |
| --- | --- | --- |
| `typescript` / `javascript` | Universal-docs template | `docs/stack/source-documentation.md` § TS/JS |
| `python` | PEP 8 / official style guide | § Python in `source-documentation.md` |
| Other | Official style guide for language | Add section; no Jarvis-hosted playbook |

## MCP and agent tooling

Jarvis does **not** ship MCP configs or framework MCP workflows.

| When | Target action |
| --- | --- |
| User confirms MCP is configured for the project | Add § MCP to `docs/stack/upstream-references.md` (tool order, when to run) |
| Optional dedicated rule | `.cursor/rules/<framework>-agent-docs.mdc` — **manual** activation; document in rules index |
| Source of truth | Vendor MCP docs + team `.cursor/mcp.json` — not Jarvis |

Template: [`upstream-references.example.md`](../templates/stack-scaffolding/upstream-references.example.md).

## Version notes

| Signal | Guidance |
| --- | --- |
| Framework major in target manifests ≠ generic examples | Prefer upstream docs for detected major; note version in `upstream-references.md` |
| Svelte 5 runes in `package.json` | Use Svelte llms.txt; do not author Svelte 3 patterns |
| Create React App vs Vite | Detect via `vite.config.*` ([`detection.md`](./detection.md)); adjust globs |
| Dual-stack at same root | **Ask** — see [`selection.md` § pause points](./selection.md#human-input-pause-points) |

## Maintenance

- Add rows when a **common** framework needs a stable upstream link set — **URLs and target artifact names only**, no Jarvis playbooks.
- Remove rows that duplicate universal-docs guidance without adding lookup value.
- Target projects extend their own `upstream-references.md`; do not symlink Jarvis.
