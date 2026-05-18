# Stack source documentation — REPLACE_WITH_PROJECT_NAME

**Extension** to [`documentation-conventions.md`](../documentation-conventions.md) §2. Fill this file after verifying language, framework, and doc tooling from the repository README and manifests.

Delete sections that do not apply. Do not invent rules for stacks this project does not use.

---

## Language and paths

| Item | Value |
| --- | --- |
| Primary language(s) | *(e.g. TypeScript)* |
| Production roots | *(e.g. `src/`)* |
| Doc lint / check command | *(verified command or “none yet”)* |

---

## Comment syntax (fill applicable sections)

### TypeScript / JavaScript

- File-level: `@fileoverview` + `@module` (logical path under production root)
- Exports: `@param`, `@returns` (omit for void), `@throws`, `@remarks`, `@see`
- Do not repeat TypeScript types in tags

### Python

- Module docstring; public functions with Args/Returns/Raises where non-obvious

### Other

*(Add Rust, Go, Java, etc. as needed)*

---

## UI components (if applicable)

*(e.g. Svelte `@component` block, React prop types with TSDoc on props interface — link official doc for the stack)*

---

## Exemptions (project-specific)

List paths or globs beyond the universal defaults (for example `**/*.spec.ts`, `**/e2e/**`).

---

## Checklist before merge

- [ ] New substantive production files follow this extension
- [ ] `pnpm run check` / equivalent doc-related command passes (if configured)
