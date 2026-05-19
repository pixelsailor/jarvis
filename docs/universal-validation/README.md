# Universal validation checklist scaffold

Generic **validation checklist** for target projects: reusable audit rows for Validator runs, human PR review, and Planner scoping, plus explicit **stack-specific extension points** so domain rows stay in one canonical file without copying Jarvis or WFD identifiers.

**Platform task:** `JR-UNIVERSAL-006`  
**Copy source:** [`../templates/universal-validation/`](../templates/universal-validation/)  
**Target layout:**

| Path | Role |
| --- | --- |
| `docs/validation-checklist.md` | Canonical checklist (from `validation-checklist.md` template) |
| `.cursor/rules/validation-checklist.mdc` | Optional — globs load when orchestration or checklist is in context |

**Related (when present):** [`../universal-pr-commit/README.md`](../universal-pr-commit/README.md) (PR template and guide link checklist rows); [`../orchestration/README.md`](../orchestration/README.md) (`JR-ORCH-001`/`002` — task folders and manifest; lifecycle ↔ checklist in checklist orchestration appendix); future `JR-ORCH-003`+ and `JR-VALIDATION-*` (artifact contracts, evidence tiers).

## When to scaffold

| Initialization path | Validation checklist |
| --- | --- |
| **Small** | Optional — skip until the project has **Accepted** ADRs or boundary rules that need cross-cutting audit IDs |
| **Medium** | Default — copy `docs/validation-checklist.md`; fill stack extension table from README § Boundaries and verified repo |
| **Large** | Required — checklist + optional `validation-checklist.mdc`; link from root README § Documentation and `.cursor/rules/index.md` Workflow row |

See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md).

## Copy and adapt (agents)

1. If the target already has `docs/validation-checklist.md`, **audit** — merge missing generic sections and extension rows; do not delete project-specific IDs without user approval.
2. Copy [`validation-checklist.md`](../templates/universal-validation/validation-checklist.md) → `docs/validation-checklist.md`.
3. Replace `REPLACE_WITH_PROJECT_NAME` and `REPLACE_WITH_VERIFY_COMMANDS` (comma-separated list from README § Development, `docs/stack/commands.md`, or manifests — see [`stack-scaffolding/commands.md`](../stack-scaffolding/commands.md)). If alignment gaps live elsewhere, update links per `adrs/GOVERNANCE.md`.
4. Fill **[Stack-specific extensions](#stack-specific-extensions-in-target)** from README boundaries and detected stack (auth, persistence, deployment, UI framework, test runners). Use verified command names only.
5. Add **ADR-specific annex** subsections when an **Accepted** ADR requires repeatable validation rows (pattern in template § ADR-specific annexes).
6. When the project uses **orchestrated** task folders, uncomment or complete § Orchestration appendix and align IDs with the project's orchestration doc (`JR-ORCH-*` on the Jarvis platform backlog).
7. Copy [`validation-checklist.mdc`](../templates/universal-validation/validation-checklist.mdc) → `.cursor/rules/validation-checklist.mdc` on **medium** (if checklist exists) or **large** path; register in `.cursor/rules/index.md` under **Workflow** with globs activation.
8. Add one line to root README § Documentation for `docs/validation-checklist.md`.
9. Update [`.github/pull_request_template.md`](../templates/universal-pr-commit/pull_request_template.md) merge-ready links if the target uses GitHub and checklist IDs are stable.
10. Add or update `PROJ-DOC-*` / `PROJ-STACK-*` rows in `docs/roadmap/backlog.md` with evidence paths.

**Handoff rule:** Generated files must not link to the Jarvis repository. All `see` targets must be target-project paths.

## Checklist ID conventions (platform default)

| Prefix | Scope | Owner |
| --- | --- | --- |
| **MG-** | Merge-ready gates (architecture-touching / orchestrated work) | Validator + human PR |
| **TST-** | Tests and evidence | Tester (primary); Validator cross-check |
| **SEC-**, **DATA-**, **AUTH-**, **API-**, **UI-**, **DEPLOY-**, **TOOL-**, … | Stack or boundary extensions | Project-defined; fill in template extension table |
| **ADR annex** | e.g. `REC-01` under a named ADR heading | Add when an Accepted ADR mandates repeatable checks |

Use stable IDs so `validation-report.md`, PR comments, and `plan.md` → Validation commands can reference them without paraphrasing.

## Relationship to other Jarvis scaffolds

| Scaffold | Relationship |
| --- | --- |
| [`../universal-adr/README.md`](../universal-adr/README.md) | Merge-ready and annex rows cite **Accepted** ADRs; alignment gaps path in **MG-01** |
| [`../universal-pr-commit/README.md`](../universal-pr-commit/README.md) | Guide §3–§4 and PR template link checklist when present |
| [`../universal-rules/README.md`](../universal-rules/README.md) | Optional `validation-checklist.mdc`; index Workflow row |
| [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) | Validator minimum read set includes checklist |
| [`../stack-scaffolding/testing.md`](../stack-scaffolding/testing.md) | **TST-** extension rows per verified test layer |
| [`../stack-scaffolding/runtime.md`](../stack-scaffolding/runtime.md) | **SEC-** / **DEPLOY-** rows for secrets and deploy boundaries |
| [`../stack-scaffolding/dependencies.md`](../stack-scaffolding/dependencies.md) | **TOOL-** rows for lockfile/script alignment |
| `JR-ORCH-001`/`002` | Task-folder path and `gate_status` keys; lifecycle ↔ **MG-*** mapping in checklist orchestration appendix |
| `JR-ORCH-003`+ (future) | `validation-report.md` template and full gate narrative in target orchestration guide |
| `JR-VALIDATION-*` (future) | Doc-only vs code init evidence; does not replace this checklist |

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- Replacing an existing `docs/validation-checklist.md` that encodes team-specific gate IDs or compliance policy.
- Renaming **MG-** or **TST-** IDs already referenced in Accepted ADRs, orchestration templates, or open `PROJ-*` tasks.
- Adding **alwaysApply** validation rules (use **globs** default in template).
- Claiming orchestrated merge-ready without `validation-report.md` when the project defines Validator stage as required.
- Inventing validation commands or test runners not present in README § Development or repo manifests.

Routine copy, placeholder replacement, extension rows from verified README/stack, and Documentation map links do not require extra approval.

## Decisions recorded for `JR-UNIVERSAL-006`

Defaults favor long-term agent efficiency; override per target when the user directs:

| Topic | Default |
| --- | --- |
| Canonical path | `docs/validation-checklist.md` |
| Cursor rule | **Optional** — `validation-checklist.mdc` with globs (orchestration folder + checklist doc); not `alwaysApply` |
| Generic rows | **MG-01**–**MG-05**, **TST-01**–**TST-05** — stable, product-agnostic wording |
| Stack-specific rows | Single **extension table** + optional themed subsections; IDs chosen per project (prefix examples in template) |
| ADR annexes | Add under **ADR-specific annexes** when an Accepted ADR needs repeatable audit items |
| Orchestration mapping | **Appendix** in checklist — example `gate_0_intake`…`gate_6_human_approval` keys; target doc links [`../orchestration/task-manifest.md`](../orchestration/task-manifest.md) |
| Second checklist file | **No** by default — split only if extension section exceeds ~80 rows (user approval) |
| WFD pattern source | Applicability table, MG/TST discipline, versioning table — no WFD product, stack, or provider identifiers |

## Stack-specific extensions in target

After copy, agents should:

1. Read README § Boundaries, § Stack, and **Accepted** ADRs.
2. For each boundary (secrets, auth, data authority, deployment, accessibility, etc.), add rows to the extension table or a named subsection.
3. Link each row to an ADR or rule path in the **Refs** column.
4. Remove unused prefix examples from the template instructions block.

Do not pre-fill WFD-specific domains (offline recipe book, Dexie, Supabase, service worker) unless the target README explicitly requires them.
