# Validation model for initialized projects

Jarvis guidance for **what to validate** when scaffolding a target project and when auditing ongoing work. This pack defines **categories** (purpose groupings); concrete audit rows live in the target [`validation-checklist.md`](../templates/universal-validation/validation-checklist.md) (**MG-***, **TST-***, extensions).

**Platform tasks:** `JR-VALIDATION-001` (categories), `JR-VALIDATION-002` (doc-only init evidence), `JR-VALIDATION-003` (runnable init evidence), `JR-VALIDATION-004`–`005` (self-containment extensions, Jarvis completion claims — planned).

## Read order (agents)

1. [`categories.md`](./categories.md) — category IDs, init vs change phases, init-path matrix
2. [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) — **VAL-EVID-00**–**05** when init produces no runnable artifact (`JR-VALIDATION-002`)
3. [`runnable-init-evidence.md`](./runnable-init-evidence.md) — **VAL-EVID-06**–**07** when init adds manifests, code, or runnable config (`JR-VALIDATION-003`)
4. Target init path — [`../orchestration/init-paths.md`](../orchestration/init-paths.md) (`JR-ORCH-005`)
5. Three gate vocabularies — [`../orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md) (`JR-ORCH-004`)
6. Checklist scaffold — [`../universal-validation/README.md`](../universal-validation/README.md) (`JR-UNIVERSAL-006`)
7. Handoff layers — [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md), [`../universal-handoff/README.md`](../universal-handoff/README.md)

## Documents

| ID | Document | Use when |
| --- | --- | --- |
| `JR-VALIDATION-001` | [`categories.md`](./categories.md) | Scoping init audits, Validator read sets, mapping `PROJ-*` to validation work |
| `JR-VALIDATION-002` | [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) | **VAL-EVID-*** tiers; doc-only init; **VAL-CAT-07** N/A |
| `JR-VALIDATION-003` | [`runnable-init-evidence.md`](./runnable-init-evidence.md) | **VAL-EVID-06**–**07**; default quality chain; **VAL-CAT-05** / **07** at runnable init |
| `JR-VALIDATION-004` | _(planned)_ | Self-containment checks beyond [`handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md) |
| `JR-VALIDATION-005` | _(planned)_ | What Jarvis may claim complete vs human approval |

## Relationship to other scaffolds

| Scaffold | Relationship |
| --- | --- |
| [`../universal-validation/README.md`](../universal-validation/README.md) | Checklist **sections** implement category rows; do not duplicate category prose in every target |
| [`../orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md) | Lifecycle gates ≠ categories; **MG-*** belong under **VAL-CAT-07** at change time |
| [`../orchestration/init-paths.md`](../orchestration/init-paths.md) | Init path selects which categories are **required** at handoff |
| [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) | README signals → `PROJ-*` → categories |
| [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | Handoff layers map to **VAL-CAT-06** + backlog honesty |

**Target ownership:** Categories are **Jarvis platform vocabulary** during init. Targets receive `docs/validation-checklist.md` (and optional rule), not a separate `validation-categories.md`, unless the user requests one on a **large** init.

## Human input (pause points)

Jarvis must **stop and ask** before:

- Replacing this category model with a target-specific taxonomy that renames **VAL-CAT-*** IDs already cited in Accepted ADRs or open `PROJ-*` tasks.
- Skipping **VAL-CAT-06** (independence) because the project is a monorepo beside Jarvis — record an explicit partial-handoff exception per [`handoff.md`](../target-roadmap/handoff.md).
- Treating initialization complete when **VAL-CAT-07** (executable evidence) is required by README or manifests but the user has not confirmed deferral.

Routine category audits during init, checklist copy, and backlog evidence updates do not require extra approval.

## Decisions recorded for `JR-VALIDATION-001`

| Topic | Default |
| --- | --- |
| Category IDs | **VAL-CAT-01**–**VAL-CAT-08** — stable platform IDs; not copied as a second checklist |
| Phases | **Initialization** (handoff) vs **change** (PR / orchestrated run) — see [`categories.md`](./categories.md) |
| Target artifact | `docs/validation-checklist.md` only; categories stay in Jarvis `docs/validation/` |
| vs gate vocabularies | Categories group *purpose*; lifecycle / **MG-*** / **PROJ-HANDOFF-*** answer *stage or claim* |
| vs checklist rows | Categories → sections + `PROJ-*`; rows (**MG-01**, **TST-01**, **SEC-01**, …) are auditable items |
| WFD | Discipline only — no WFD product, provider, or gap identifiers |
