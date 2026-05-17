# Project roadmap backlog

**Last updated:** 2026-05-17

Stable `PROJ-*` IDs support stop/resume across agent sessions. See [`README.md`](./README.md) for folder purpose.

---

## PROJ-README — Target README

- [x] `PROJ-README-001`: Draft root README with purpose, audience, stack summary, and boundaries. **required for handoff**
- [ ] `PROJ-README-002`: Add Documentation section linking `docs/roadmap/README.md` and planned ADR index.

## PROJ-ADR — Architecture decisions

- [ ] `PROJ-ADR-001`: Add `adrs/` with `INDEX.md`, `GOVERNANCE.md`, and `TEMPLATE.md`. **required for handoff**
  - Depends on: `PROJ-README-001`
- [ ] `PROJ-ADR-002`: Record first Accepted ADR for data ownership or deployment boundary (project-specific).

## PROJ-RULE — Agent guidance

- [ ] `PROJ-RULE-001`: Create `.cursor/rules/` layout and `index.md` routing table.
- [ ] `PROJ-RULE-002`: Add always-apply rule for documentation conventions.

## PROJ-DOC — Documentation conventions

- [ ] `PROJ-DOC-001`: Add documentation conventions under `docs/` (outside `docs/roadmap/`).

## PROJ-AGENT — Agent contracts

- [ ] `PROJ-AGENT-001`: *(optional for small projects)* Add agent roster only if using orchestrated multi-step setup.

## PROJ-ORCH — Orchestration

- [ ] `PROJ-ORCH-001`: *(optional)* Add task-folder template if setup needs Plan–Build–Test–Validate artifacts.

## PROJ-STACK — Stack-specific setup

- [ ] `PROJ-STACK-001`: Document package manager and validation commands from actual project files (do not invent scripts).
  - Evidence: `package.json` inspected on 2026-05-17.
- [ ] `PROJ-STACK-002`: Add framework-specific rule or best-practices doc.

## PROJ-HANDOFF — Self-containment

- [ ] `PROJ-HANDOFF-001`: Confirm no generated doc links to the Jarvis repository. **required for handoff**
- [ ] `PROJ-HANDOFF-002`: Confirm rules and agents cite only target-project paths. **required for handoff**
- [ ] `PROJ-HANDOFF-003`: User acknowledges remaining non-handoff tasks before normal feature development.

---

## How agents use this file

1. Read the root README, then `docs/roadmap/README.md`, then this backlog.
2. Pick the lowest open `PROJ-*` task whose dependencies are satisfied.
3. On completion, check the box; add a one-line sub-bullet only when evidence or follow-up is non-obvious.
4. When the target README changes scope, add new `PROJ-*` rows in the matching section.
