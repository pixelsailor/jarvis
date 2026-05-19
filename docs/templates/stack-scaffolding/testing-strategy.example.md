# Testing strategy — REPLACE_WITH_PROJECT_NAME

**Purpose:** Map test **layers** (unit, integration, component, browser, e2e) to **verified commands** and folder layout. Default handoff command stays in root README § Development.

**Jarvis:** Copy to `docs/stack/testing-strategy.md` during initialization (`JR-STACK-004`). Do not link to the Jarvis repository.

**Last verified:** YYYY-MM-DD  
**Primary package path:** `.`  
**Evidence:** *(e.g. `vitest.config.ts`, `package.json`, `playwright.config.ts`)*

---

## Default handoff command

Agents run this for routine code changes (must match README § Development and `docs/stack/commands.md`):

```bash
REPLACE_WITH_DEFAULT_TEST_COMMAND
```

---

## Layers

| Layer | In scope? | Command (verified) | Location / notes |
| --- | --- | --- | --- |
| unit | yes | `pnpm run test` | Colocated `src/**/*.test.ts` |
| integration | no | — | |
| component | optional | | |
| browser | no | — | |
| e2e | optional | `pnpm run test:e2e` | `e2e/` — release gate only |

*(Delete rows for layers not used. Commands must exist in `package.json` or documented CI/Makefile.)*

---

## CI vs local (optional)

| Context | Command | Notes |
| --- | --- | --- |
| Local handoff | `pnpm run test` | |
| CI | | same / different |

---

## Related target docs

| Doc | Role |
| --- | --- |
| `docs/stack/commands.md` | All verified script invocations |
| `docs/stack/stack-profile.md` | Test runner names |
| `docs/validation-checklist.md` | **TST-** rows per in-scope layer |
| Root `README.md` § Development | Short default test line |

---

## Checklist (handoff)

- [ ] Every command maps to a manifest script or documented CI target
- [ ] No invented runners or folders
- [ ] `PROJ-STACK-003` marked complete with evidence when test tooling exists
