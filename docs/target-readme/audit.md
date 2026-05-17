# Auditing an existing target README

When a root `README.md` already exists, Jarvis **audits before editing**. Goal: preserve intentional product language, fill agent-critical gaps, and avoid turning the README into a migration battleground.

## When to audit vs treat as missing

| Condition | Action |
| --- | --- |
| No `README.md` | [`intake-questions.md`](./intake-questions.md) |
| Placeholder only ("TODO", template boilerplate, wrong project name) | Intake + replace |
| Substantive README (> ~30 lines of project-specific content) | Audit |
| Multiple READMEs (`README.md` + `README.*.md`) | Confirm canonical root with user if ambiguous |

## Audit procedure (agent steps)

1. **Read** root `README.md` end-to-end.
2. **Scan** repo for contradictions: `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, existing `adrs/`, `.cursor/rules/`, `docs/`.
3. **Score** each section in [`outline.md`](./outline.md) (see rubric).
4. **Classify gaps** (see gap classes).
5. **Produce an audit artifact** in the target session (comment to user or short section in chat): gap list, proposed edits, contradictions, questions for human.
6. **Apply edits** only after user confirms **or** when edits are purely additive/routing (Documentation links, roadmap pointer, verified Development commands) and do not change principles.

## Rubric

For each outline section, assign one status:

| Status | Meaning |
| --- | --- |
| **Present** | Section exists with project-specific content meeting outline intent |
| **Weak** | Mentioned but too vague for agents to scaffold (e.g. "modern stack") |
| **Missing** | No equivalent section |
| **Misplaced** | Content exists but belongs in ADR, rules, or `docs/` (candidate to move + link) |
| **Stale** | Contradicts repo files or user-stated facts |
| **Harmful** | Jarvis links, invented commands, or secrets — fix immediately with user notice |

### Required sections checklist

Copy into audit notes:

```markdown
- [ ] 1. Title and summary — Present / Weak / Missing / Stale
- [ ] 2. Principles — ...
- [ ] 3. Core capabilities — ...
- [ ] 4. Technology stack — ...
- [ ] 5. Architecture boundaries — ...
- [ ] 6. Documentation (incl. docs/roadmap during setup) — ...
- [ ] 7. Development (verified commands only) — ...
```

Conditional sections: note **N/A** or score separately (8–11).

## Gap classes and response

| Class | Examples | Jarvis action |
| --- | --- | --- |
| **A — Scaffold blockers** | No principles; no stack; boundaries contradict `adrs/` | Propose README edits; add `PROJ-README-*`; pause if principles unclear |
| **B — Routing gaps** | No Documentation map; no roadmap link during init | Add links; update `PROJ-README-*` |
| **C — Misplaced depth** | Full API spec, rule text, ADR bodies in README | Move plan: create `PROJ-DOC-*` / `PROJ-ADR-*`; shorten README to summary + link |
| **D — Stale facts** | README says React; repo is Svelte | Propose correction; cite file evidence; ask if migration in progress |
| **E — Harmful** | Jarvis-relative links; committed secrets; invented scripts | Fix with user visibility; never hand off harmful content |

## Human pause (mandatory)

Ask before:

- Rewriting or removing **Principles** or **Architecture boundaries** that read as intentional product decisions.
- Resolving **Stale** vs **in-progress migration** (README ahead of code or code ahead of README).
- Merging two products' READMEs or changing **audience** (Q2 from intake).

## Contradiction handling

| Situation | Default |
| --- | --- |
| README vs lockfiles | Trust lockfiles; propose README stack update |
| README vs Accepted ADR | Trust ADR; propose README alignment |
| README vs unmerged code on branch | Note branch context; ask if README describes target or current |
| README vs user message in chat | Ask user; chat is not durable |

## Audit output template

Use in PR description or session summary:

```markdown
## README audit — [project name]

**Canonical file:** README.md  
**Init path:** small | medium | large

### Section scores
| Section | Status | Notes |
| --- | --- | --- |
| Summary | Present | ... |

### Gap class summary
- A (blockers): ...
- B (routing): ...
- C (misplaced): ...
- D (stale): ...
- E (harmful): ...

### Proposed edits
1. ...

### Questions for human
- ...
```

## After audit

1. Apply agreed README changes per [`outline.md`](./outline.md).
2. Run [`scaffolding-map.md`](./scaffolding-map.md) to refresh `PROJ-*` tasks.
3. Before handoff, run [`handoff-checklist.md`](./handoff-checklist.md).
