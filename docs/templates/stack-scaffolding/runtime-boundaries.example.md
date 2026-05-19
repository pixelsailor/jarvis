# Runtime and environment boundaries — REPLACE_WITH_PROJECT_NAME

**Purpose:** Record deployment target, execution model, and **env var names** (never values) for client vs server boundaries.

**Jarvis:** Copy to `docs/stack/runtime-boundaries.md` during initialization (`JR-STACK-005`). Do not link to the Jarvis repository.

**Last verified:** YYYY-MM-DD  
**Primary package path:** `.`  
**Evidence:** *(e.g. `svelte.config.js`, `netlify.toml`, `.env.example`)*

---

## Runtime and deployment

| Item | Value |
| --- | --- |
| Language runtime | *(e.g. Node.js 20 — from `engines` / `.nvmrc`)* |
| Framework adapter / platform | *(e.g. SvelteKit + adapter-netlify; Netlify)* |
| Execution model | *(e.g. serverless functions + static assets)* |
| Production deploy | *(e.g. Netlify connected branch `main`)* |

---

## Client vs server code

| Area | Boundary |
| --- | --- |
| Server-only modules | `src/lib/server/**`, `+server.ts`, hooks.server |
| Public env prefix | `PUBLIC_` *(or project-specific — from docs)* |
| Secrets | Never in client bundle; server routes and platform env only |

---

## Environment variables (names only)

| Name | Class | Used by | Notes |
| --- | --- | --- | --- |
| `PUBLIC_APP_URL` | public | client | |
| `DATABASE_URL` | secret | server | |

*(Source: `.env.example` and README — do not paste values.)*

---

## `.env.example` discipline

- [ ] `.env.example` lists all required names for local dev
- [ ] Secrets are not committed in `.env`
- [ ] Public vars use the framework’s public prefix

---

## Related ADRs and rules

| Doc | Role |
| --- | --- |
| `adrs/ADR-NNN-*.md` | *(link Accepted ADRs for secrets / data authority)* |
| `.cursor/rules/*` | Rules cite ADR + this file |
| Root `README.md` § Boundaries | Summary aligned with this file |

---

## Related target docs

| Doc | Role |
| --- | --- |
| `docs/stack/stack-profile.md` | Runtime and deployment hint summary |
| `docs/stack/commands.md` | Prerequisites / runtime version |

---

## Checklist (handoff)

- [ ] Adapter/platform matches manifests
- [ ] No secret values in generated docs
- [ ] README § Boundaries agrees with this file
- [ ] `PROJ-STACK-004` marked complete with evidence when deploy/secrets apply
