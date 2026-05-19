# Project roadmap backlog

**Last updated:** 2026-05-17

Stable `PROJ-*` IDs support stop/resume across agent sessions. Field rules: [`../../target-roadmap/conventions.md`](../../target-roadmap/conventions.md). See [`README.md`](./README.md) for folder purpose.

---

## PROJ-README — Target README

- [x] `PROJ-README-001`: Draft root README with purpose, audience, stack summary, and boundaries. **required for handoff**
  - Evidence: root `README.md` matches [`../../target-readme/outline.md`](../../target-readme/outline.md) required sections; audited 2026-05-17.
- [ ] `PROJ-README-002`: Add Documentation section linking `docs/roadmap/README.md` and planned ADR index.
  - Depends on: `PROJ-README-001`
  - Owner: agent

## PROJ-ADR — Architecture decisions

- [ ] `PROJ-ADR-001`: Add `adrs/` with `INDEX.md`, `GOVERNANCE.md`, and `TEMPLATE.md`. **required for handoff**
  - Depends on: `PROJ-README-001`
  - Owner: agent
- [ ] `PROJ-ADR-002`: Record first Accepted ADR for data ownership or deployment boundary (project-specific).
  - Depends on: `PROJ-ADR-001`

## PROJ-RULE — Agent guidance

- [ ] `PROJ-RULE-001`: Create `.cursor/rules/` layout and `index.md` routing table.
  - Owner: agent
- [ ] `PROJ-RULE-002`: Add always-apply rule for documentation conventions.
  - Depends on: `PROJ-RULE-001`

## PROJ-DOC — Documentation conventions

- [ ] `PROJ-DOC-001`: Add documentation conventions under `docs/` (outside `docs/roadmap/`).

## PROJ-AGENT — Agent contracts

*(No active tasks — see **Cancelled** for a declined optional row.)*

## PROJ-ORCH — Orchestration

*(No active tasks — see **Deferred**.)*

## PROJ-STACK — Stack-specific setup

- [x] `PROJ-STACK-001`: Document package manager and validation commands from actual project files (do not invent scripts). **required for handoff**
  - Owner: agent
  - Evidence: `package.json` inspected 2026-05-17; `docs/stack/commands.md`; README § Development aligned ([`commands.md`](../../stack-scaffolding/commands.md)).
- [ ] `PROJ-STACK-002`: Add framework-specific rules and stack docs from confirmed capabilities ([`selection.md`](../../stack-scaffolding/selection.md)).
  - Depends on: `PROJ-STACK-000` (stack-profile)
  - Owner: agent
  - Deliverables: `.cursor/rules/<framework>-conventions.mdc`, `docs/stack/upstream-references.md`; optional `docs/stack/framework-guide.md`
- [ ] `PROJ-STACK-003`: Document testing layers and verified test commands ([`testing.md`](../../stack-scaffolding/testing.md)). **required for handoff** when test tooling exists
  - Depends on: `PROJ-STACK-001`
  - Deliverables: `docs/stack/testing-strategy.md`
- [ ] `PROJ-STACK-004`: Document runtime, deployment, and secrets/env boundaries ([`runtime.md`](../../stack-scaffolding/runtime.md)). **required for handoff** when deploy adapter or secrets split exists
  - Deliverables: `docs/stack/runtime-boundaries.md`; align README § Boundaries
- [ ] `PROJ-STACK-005`: Dependency and tooling review — no automatic upgrades ([`dependencies.md`](../../stack-scaffolding/dependencies.md)). *(optional for handoff)*
  - Depends on: `PROJ-STACK-001`
  - Deliverables: optional `docs/stack/dependency-review.md`; align `stack-profile` and `commands.md`

## PROJ-HANDOFF — Self-containment

- [ ] `PROJ-HANDOFF-001`: Confirm no generated doc links to the Jarvis repository. **required for handoff**
  - Run: `docs/handoff-self-containment.md` **IND-01**–**IND-06** (copy from Jarvis [`universal-handoff`](../../universal-handoff/README.md) during init)
  - Owner: agent
- [ ] `PROJ-HANDOFF-002`: Confirm rules and agents cite only target-project paths. **required for handoff**
  - Run: same checklist **IND-07**–**IND-09**
  - Depends on: `PROJ-RULE-001`
  - Owner: agent
- [ ] `PROJ-HANDOFF-003`: User acknowledges remaining non-handoff tasks before normal feature development. **required for handoff**
  - Owner: human

---

## How agents use this file

1. Read the root README, then `docs/roadmap/README.md`, then this backlog.
2. Follow the project's backlog conventions (copy from Jarvis [`target-roadmap/conventions.md`](../../target-roadmap/conventions.md) during init if not vendored into the target repo).
3. Pick the lowest open `PROJ-*` task whose dependencies are satisfied and that has no `Blocker:` sub-bullet.
4. On completion, check the box; add `Evidence:` when the task is **required for handoff** or verification is non-obvious.
5. When the target README changes scope, add new `PROJ-*` rows in the matching section.
6. To defer or cancel, move the row to **Deferred** or **Cancelled** below — do not leave it in an active section.

---

## Deferred

- [ ] `PROJ-ORCH-001`: Add task-folder template if setup needs Plan–Build–Test–Validate artifacts.
  - Deferred: resume after universal orchestration templates exist in Jarvis (`JR-ORCH-*`).
  - Owner: agent

---

## Cancelled

- [ ] `PROJ-AGENT-001`: Add agent roster for orchestrated setup.
  - Cancelled: small init path selected; user confirmed no agent roster at setup.
  - Owner: human
