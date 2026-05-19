# Plan — REPLACE_WITH_TASK_ID

Replace `REPLACE_WITH_TASK_ID` with manifest `task_id`. Delete HTML comments before handoff. Required section order is fixed — see target `.cursor/agents/planner.md` and orchestration artifact ownership guide.

## Objective restatement

<!-- One sentence: what "done" looks like for the user or system. -->

## Risk and phase strategy

<!-- Manifest risk_tier (small | medium | large), phase count, skipped-stage assumptions, change budget. -->

## Scope boundary

### In scope

-

### Out of scope

-

## Phases

<!-- Bounded slices the Builder can implement in order. One phase per remediation loop when possible. -->

| Phase | Goal | Done when |
| ----- | ---- | --------- |
| 1     |      |           |
| 2     |      |           |

### Phase 1 — (title)

- **Deliverables:**
- **Files touched:** (see Component/file map)
- **Dependencies:** (prior phases, flags, or human decisions)

### Phase 2 — (title)

- **Deliverables:**
- **Files touched:**
- **Dependencies:**

## Component/file map

<!-- Every path to create or modify, with purpose. Align with phases above. -->

| Path | Action | Purpose |
| ---- | ------ | ------- |
|      | create / modify |         |

## Interface contracts

<!-- APIs, props, schemas, store contracts — only what Builder must implement. -->

## ADR references

<!-- Accepted ADRs only. State implication for this task, not pointer-only text. -->

- **ADR-NNN:** …

## Validation commands

<!-- Exact commands from docs/stack/commands.md or README § Development — do not invent scripts. Map to AC IDs where helpful. Pull applicable rows from docs/validation-checklist.md when the task touches those domains. -->

| Step | Owner | Command or check | Pass criteria |
| ---- | ----- | ---------------- | ------------- |
| Lint / types | Builder / Validator |  |  |
| Tests | Tester |  | AC-… in test-report.md |
| Manual QA | Human |  | AC-… |

## Risks

| Risk | Likelihood | Impact | Mitigation |
| ---- | ---------- | ------ | ---------- |
|      | low / med / high |        |            |

## Rollback

<!-- How to undo this change set safely. -->

- **Code:** …
- **Data / config:** …

## Open questions

<!-- Unresolved items. Builder must not invent answers; record under build-log Unresolved open questions. -->

- [ ] …
