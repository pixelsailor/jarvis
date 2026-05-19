# Toolchain commands — REPLACE_WITH_PROJECT_NAME

**Purpose:** Verified install and quality commands for agents and contributors. Root README § **Development** summarizes the default chain; this file is the detailed record with evidence.

**Jarvis:** Copy to `docs/stack/commands.md` during initialization (`JR-STACK-003`). Do not link to the Jarvis repository.

**Last verified:** YYYY-MM-DD  
**Primary package path:** `.` *(monorepo: e.g. `apps/web`)*  
**Package manager:** *(e.g. pnpm — from lockfile)*  
**Evidence:** *(e.g. `package.json`, `pnpm-lock.yaml`, `Makefile`)*

---

## Prerequisites

| Requirement | Source |
| --- | --- |
| Node.js | `package.json` `engines.node` *(or `.nvmrc`)* |
| | |

---

## Install

```bash
# From repository root (or primary package path)
REPLACE_WITH_INSTALL_COMMAND
```

---

## Scripts (verified)

| Role | Script key | Invocation | Notes |
| --- | --- | --- | --- |
| dev | `dev` | `pnpm run dev` | |
| build | `build` | `pnpm run build` | |
| check | `check` | `pnpm run check` | typecheck / svelte-check |
| lint | `lint` | `pnpm run lint` | |
| test | `test` | `pnpm run test` | |
| format | `format` | `pnpm run format` | optional daily use |

*(Delete rows for scripts that do not exist. Add rows for extra keys — do not invent keys.)*

---

## Default agent / handoff chain

Commands agents and Validator should run for typical code changes (also copied to README § Development):

```bash
REPLACE_WITH_CHECK_COMMAND
REPLACE_WITH_LINT_COMMAND
REPLACE_WITH_TEST_COMMAND
```

**Execution log (optional):** YYYY-MM-DD — check/lint/test passed locally.

---

## CI mapping (optional)

| CI job / step | Command | Same as local? |
| --- | --- | --- |
| | | yes / no |

---

## Monorepo notes (optional)

| Package path | Install | Dev | Quality |
| --- | --- | --- | --- |
| `apps/web` | `pnpm install` | `pnpm run dev` | `pnpm run check` |

---

## Related target docs

| Doc | Role |
| --- | --- |
| `docs/stack/stack-profile.md` | Capabilities and package manager name |
| Root `README.md` § Development | Short onboarding copy |
| `docs/validation-checklist.md` | `REPLACE_WITH_VERIFY_COMMANDS` |
| `.cursor/rules/` | May cite script keys; must match this file |

---

## Checklist (handoff)

- [ ] Every invocation maps to a manifest script or documented make/CI target
- [ ] README § Development matches the default chain above
- [ ] No links to Jarvis
- [ ] `PROJ-STACK-001` marked complete with evidence in `docs/roadmap/backlog.md`
