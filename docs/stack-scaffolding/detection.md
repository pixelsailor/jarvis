# Stack detection from repository files

Jarvis infers language, framework, package manager, and common adjuncts from **observable files** in the target project. Output is a **draft capability set** with per-field confidence — not final until [`confirmation.md`](./confirmation.md) runs.

**Prerequisite:** Know the target repository root (and, for monorepos, the primary package path — see [Monorepos](#monorepos)).

## Detection pass (agent steps)

1. **Set scope** — default: repository root. If `pnpm-workspace.yaml`, `lerna.json`, `nx.json`, or `turbo.json` exists, list workspace members before inferring product stack.
2. **Scan signals** — walk tables below from the scoped root; record **path evidence** for each inferred value.
3. **Assign confidence** per field: `high` | `medium` | `low` (rules in [Confidence](#confidence)).
4. **Compare README** — if root `README.md` has § Technology Stack, note match or mismatch; mismatches are **conflicts** (confirmation required).
5. **Hand off** — pass draft + evidence to [`confirmation.md`](./confirmation.md).

Do not copy commands, versions, or framework playbooks in this step — detection only names what the repo **is**.

## Confidence

| Level | Meaning | Agent action |
| --- | --- | --- |
| **high** | Lockfile + manifest agree, or unambiguous single-framework layout | May write to stack-profile after user **confirmation batch** (user can correct silently) |
| **medium** | Strong file signal but no lockfile, or version range only in README | List as **assumption** in confirmation batch |
| **low** | Heuristic only (file extension counts, one devDependency) | **Ask** before recording |
| **conflict** | Two incompatible high signals, or README ≠ manifests | **Ask** before recording |

**Rule:** Inference fills **capabilities**, not **product principles**. If stack choice implies architecture (e.g. "static site only"), that belongs in README § Principles or ADRs — not inferred from a single config file.

## Capability fields

Record these in the draft (target template: [`stack-profile.example.md`](../templates/stack-scaffolding/stack-profile.example.md)):

| Field | Description | Example values |
| --- | --- | --- |
| `primary_language` | Main implementation language | TypeScript, Python, Go, Rust, PHP, Ruby, Java, C# |
| `secondary_languages` | Other languages with substantial code | SQL, Shell, CSS |
| `framework` | App or UI framework | SvelteKit, Next.js, Angular, Django, Rails |
| `runtime` | Execution environment when distinct from language | Node.js, Deno, Bun, JVM, .NET, PHP-FPM |
| `package_manager` | Dependency tool for that language ecosystem | pnpm, npm, yarn, pip, uv, cargo, go modules |
| `ui_library` | Component/CSS layer when present | Tailwind CSS, MUI, shadcn |
| `datastore` | Primary durable store if evident | PostgreSQL, SQLite, Dexie, Supabase client-only |
| `test_runners` | Detected test tools (names only) | Vitest, Jest, Playwright, pytest |
| `deployment_hint` | Hosting adapter or platform signal | Netlify, Vercel, Docker, static export |

Omit fields with no evidence. Use `unknown` only in the draft worksheet — do not write `unknown` into the target README.

## Language signals

| Signal | Infers | Confidence |
| --- | --- | --- |
| `tsconfig.json` / `jsconfig.json` | TypeScript or JavaScript project | high for TS if `"strict"` or `.ts` dominant under `src/` |
| `*.ts` / `*.tsx` majority under production root | TypeScript | medium without `tsconfig` |
| `pyproject.toml` / `requirements.txt` / `Pipfile` | Python | high |
| `go.mod` | Go | high |
| `Cargo.toml` | Rust | high |
| `composer.json` | PHP | high |
| `Gemfile` | Ruby | high |
| `pom.xml` / `build.gradle` / `build.gradle.kts` | Java / Kotlin (JVM) | high |
| `*.csproj` / `*.sln` | C# / .NET | high |
| `Package.swift` | Swift | high |

## Framework and meta-framework signals

| Signal | Infers | Notes |
| --- | --- | --- |
| `@sveltejs/kit` in `package.json` | SvelteKit | Prefer over plain `svelte` |
| `svelte` without kit | Svelte (SPA or library) | medium if no routes dir |
| `next` in dependencies | Next.js | |
| `nuxt` / `nuxt.config.*` | Nuxt | |
| `astro` / `astro.config.*` | Astro | |
| `@angular/core` | Angular | |
| `react` + `react-dom` | React | Distinguish CRA vs Vite via build tool |
| `vite.config.*` + `react` | React + Vite | |
| `vue` / `nuxt` | Vue / Nuxt | |
| `remix` / `@remix-run/*` | Remix | |
| `expo` / `react-native` | React Native / Expo | |
| `django` in Python deps | Django | |
| `fastapi` | FastAPI | |
| `flask` | Flask | |
| `rails` gem | Ruby on Rails | |
| `wp-config.php` / WordPress tree | WordPress | Confirm intentional product vs theme-only |
| `svelte.config.js` + `src/routes` | SvelteKit layout likely | supporting |

## Package manager signals

| Signal | Infers | Confidence |
| --- | --- | --- |
| `pnpm-lock.yaml` | pnpm | high |
| `package-lock.json` | npm | high |
| `yarn.lock` | Yarn classic or Berry | high; check `.yarnrc.yml` for Berry |
| `bun.lock` / `bunfig.toml` | Bun | high |
| `uv.lock` | uv (Python) | high |
| `poetry.lock` | Poetry | high |
| Only `package.json` | npm **candidate** | low — ask if user will add a lockfile |

**Do not ask** package manager when a lockfile exists ([`intake-questions.md`](../target-readme/intake-questions.md)).

## Datastore and backend signals

| Signal | Infers |
| --- | --- |
| `prisma/schema.prisma` | Prisma → note provider (postgresql, sqlite, …) |
| `drizzle.config.*` | Drizzle ORM |
| `supabase` in dependencies + config | Supabase client usage (not necessarily hosted DB) |
| `dexie` | IndexedDB via Dexie |
| `mongoose` / `mongodb` driver | MongoDB |
| `redis` client libs | Redis (cache or primary — ask if architectural) |
| `docker-compose.yml` services | Hint only — read service images |

## Test and quality signals

| Signal | Infers |
| --- | --- |
| `vitest` in devDependencies / `vitest.config.*` | Vitest |
| `jest` / `jest.config.*` | Jest |
| `@playwright/test` | Playwright |
| `cypress` | Cypress |
| `pytest.ini` / `pytest` in pyproject | pytest |
| `testing-library` packages | Component/integration style (record name only) |

Commands are **[`commands.md`](./commands.md)** (`JR-STACK-003`) — detection may **name** runners here, not invent `pnpm test` scripts.

## Deployment and runtime signals

| Signal | Infers |
| --- | --- |
| `@sveltejs/adapter-netlify` etc. | Deployment target hint |
| `vercel.json` | Vercel |
| `netlify.toml` | Netlify |
| `Dockerfile` | Container deploy (record base image if obvious) |
| `serverless.yml` / SAM template | Serverless |

## Monorepos

When workspace tooling is present:

1. List member paths from workspace config.
2. **Do not assume** the repo root is the product — see intake Q8 ([`intake-questions.md`](../target-readme/intake-questions.md)).
3. Run the detection pass **per candidate root** the user names, or per README path hints (`apps/web`, `packages/ui`).
4. Record `monorepo: true` and `primary_package_path` in stack-profile after confirmation.

**Pause** if more than one app package has equal weight (two `package.json` with `name` and `dev` scripts) and the user has not identified the product.

## Paths to deprioritize

Do not treat these as the primary stack unless the user confirms:

| Path pattern | Why |
| --- | --- |
| `node_modules/`, `vendor/`, `dist/`, `build/` | Generated or third-party |
| `examples/`, `samples/`, `demo/` | Tutorial noise |
| `docs/` static site only | May differ from main app |
| Another package inside a monorepo | Not the product root |

## Legacy and template repos

| Situation | Behavior |
| --- | --- |
| Empty repo / only LICENSE | No detection — use intake Q5; stack-profile deferred |
| Generic template (`REPLACE_ME`, sample app name) | Treat README as low trust; prefer manifests |
| Jarvis workspace contains WFD or other reference project | **Never** copy that stack into an unrelated target |

## Output worksheet (Jarvis session — not committed)

Use a short table while detecting; commit only after confirmation:

```text
Field                 | Value           | Confidence | Evidence paths
----------------------|-----------------|------------|------------------
primary_language      | TypeScript      | high       | tsconfig.json, src/**/*.ts
framework             | SvelteKit       | high       | package.json @sveltejs/kit
package_manager       | pnpm            | high       | pnpm-lock.yaml
...
```

## WFD and Jarvis legacy

What's For Dinner and other reference repos may appear in the same workspace. **Detection runs on the target path only.** WFD is a discipline template, not automatic stack evidence ([root README](../../README.md)).

Jarvis `frameworks/*` and `libraries/*` are **not** scanned during detection — they are copy sources after capabilities are confirmed ([`selection.md`](./selection.md), [`source-registry.md`](./source-registry.md); legacy review `JR-STACK-007`).
