# Target README handoff checklist

Use when README work is complete enough to claim **README self-containment** — the target README no longer depends on Jarvis, chat context, or unlinked promises.

This checklist is **README-scoped**. Full project handoff also requires `PROJ-HANDOFF-*` in `docs/roadmap/backlog.md` and non-README artifacts (ADRs, rules, etc.) per [`platform-spec.md`](../roadmap/platform-spec.md#handoff-checklist-for-future-jarvis-output).

## When to run

- Before marking `PROJ-README-*` complete
- Before Jarvis declares initialization handoff
- After any late README edit that touched Principles, Stack, or Boundaries

## README self-containment checks

### Independence

- [ ] **No Jarvis URLs or paths** — README does not link to the Jarvis repo, `JR-*` tasks, or Jarvis-only templates as required reading.
- [ ] **No chat-only promises** — Every "we will" or "planned" item has a `PROJ-*` task or is removed.
- [ ] **Standalone comprehension** — A cold agent can state project purpose, audience, stack, and boundaries from the README alone.

### Structure (see [`outline.md`](./outline.md))

- [ ] **Required sections present** — Summary, Principles, Core capabilities, Technology stack, Architecture boundaries, Documentation, Development (if repo has tooling).
- [ ] **No playbook dump** — Framework tutorials, full rule text, and ADR bodies are not in the README.
- [ ] **Conditional sections** — Data ownership / roadmap / structure included only when applicable (no empty "TBD" sections).

### Accuracy

- [ ] **Stack matches repo** — Stack section agrees with lockfiles and primary config (`package.json`, `pyproject.toml`, etc.).
- [ ] **Development commands verified** — Every command in § Development was run or taken verbatim from manifest; none invented.
- [ ] **Env vars** — Names only; correct client/server/CI attribution; no secret values.
- [ ] **ADR alignment** — README does not contradict Accepted ADRs; if drift exists, `docs/readme-adr-alignment-gaps.md` or equivalent is linked (project may add this later via `PROJ-DOC-*`).

### Routing

- [ ] **Documentation map** — All linked paths exist **or** have open `PROJ-*` tasks with stable IDs.
- [ ] **Setup backlog** — During init, `docs/roadmap/README.md` is linked; after full handoff, README either keeps it (ongoing foundation work) or points to archived/completed state per user choice.
- [ ] **Rules and ADRs** — README points to `adrs/INDEX.md` and `.cursor/rules/` when those exist; does not duplicate their content.

### Provenance (optional)

- [ ] **Provenance** — If mentioned, Jarvis is credited as **historical** setup assistance in `docs/roadmap/` or CONTRIBUTING, not as a runtime dependency in the root README.

## Evidence to record (optional sub-bullets)

On `PROJ-README-*` completion, add sub-bullets only when non-obvious:

```markdown
- [x] `PROJ-README-001`: Draft root README ...
  - Handoff checklist passed 2026-05-17; `pnpm run check` verified from package.json.
```

## Failures → actions

| Failure | Action |
| --- | --- |
| Jarvis link in README | Remove or replace with target-local doc; `PROJ-HANDOFF-001` |
| Invented script | Remove from Development; add `PROJ-STACK-*` with evidence task |
| Weak principles | Return to [`intake-questions.md`](./intake-questions.md) Q4 or audit class A |
| Broken Documentation link | Create file or `PROJ-DOC-*`; do not mark README done |
| README vs ADR conflict | Fix README or ADR via human decision; track gap doc if needed |

## Relationship to `PROJ-HANDOFF-*`

| Checklist | Scope |
| --- | --- |
| **This file** | Root `README.md` quality and independence |
| **`PROJ-HANDOFF-*` in backlog** | Whole-repo self-containment (rules cite target paths, no Jarvis in generated docs, user sign-off) |

Both must pass before Jarvis tells the user initialization is complete.

## Post-handoff README maintenance

Target project owners may:

- Trim the Documentation map entry for `docs/roadmap/` if setup is finished and backlog archived.
- Add product roadmap links without reopening Jarvis.
- Require README updates when Accepting ADRs that change boundaries — same discipline as [`scaffolding-map.md`](./scaffolding-map.md#when-readme-changes--update-backlog).

Jarvis is not required for those updates.
