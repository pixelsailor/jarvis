# Jarvis platform backlog

Resumable platform buildout tasks. Stable reference material lives in [`platform-spec.md`](./platform-spec.md). Update rules: [`MAINTENANCE.md`](./MAINTENANCE.md).

## Root positioning

- [x] `JR-README-001`: Rewrite Jarvis's root README around the new project-foundation mission.
- [x] `JR-README-002`: Add a refactor notice warning that old files may be stale or legacy.
- [x] `JR-README-003`: Clarify that WFD is a conceptual template, not stack-specific source material to copy.
- [x] `JR-README-004`: State that target projects must be independent after handoff.
- [ ] `JR-README-005`: Revisit the README after core docs exist and replace transition language with stable documentation links.

## Platform todo and roadmap

- [x] `JR-TODO-001`: Create a Jarvis-owned roadmap/todo document for the repurpose.
- [x] `JR-TODO-002`: Distinguish Jarvis's own README from target-project READMEs.
- [x] `JR-TODO-003`: Capture the target-project independence requirement.
- [x] `JR-TODO-004`: Decide whether this roadmap remains a single file or moves into a structured `docs/roadmap/` area.
  - Structured `docs/roadmap/` with spec, backlog, open-decisions, and maintenance docs; see [`README.md`](./README.md).
- [x] `JR-TODO-005`: Add maintenance rules for when agents update this file.
  - See [`MAINTENANCE.md`](./MAINTENANCE.md).

## Target README workflow

- [x] `JR-TARGET-README-001`: Define the canonical target README outline Jarvis should generate.
  - [`docs/target-readme/outline.md`](../target-readme/outline.md)
- [x] `JR-TARGET-README-002`: Define the minimum questions Jarvis asks when no target README exists.
  - [`docs/target-readme/intake-questions.md`](../target-readme/intake-questions.md)
- [x] `JR-TARGET-README-003`: Define how Jarvis audits an existing target README before editing.
  - [`docs/target-readme/audit.md`](../target-readme/audit.md)
- [x] `JR-TARGET-README-004`: Define how the target README drives downstream ADR, rule, doc, and agent scaffolding.
  - [`docs/target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md)
- [x] `JR-TARGET-README-005`: Define a handoff checklist for target README self-containment.
  - [`docs/target-readme/handoff-checklist.md`](../target-readme/handoff-checklist.md); index [`docs/target-readme/README.md`](../target-readme/README.md)

## Target project todo workflow

- [x] `JR-TARGET-TODO-001`: Choose the default filename and location for the target-project roadmap.
  - Default: `docs/roadmap/` (`README.md` + `backlog.md`, `PROJ-*` IDs); examples in [`../../templates/target-project-roadmap/`](../../templates/target-project-roadmap/); spec in [`platform-spec.md`](./platform-spec.md#default-location-and-layout).
- [x] `JR-TARGET-TODO-002`: Define the todo schema or Markdown conventions.
  - [`docs/target-roadmap/conventions.md`](../target-roadmap/conventions.md); index [`docs/target-roadmap/README.md`](../target-roadmap/README.md).
- [x] `JR-TARGET-TODO-003`: Define required fields such as ID, status, dependency, owner, evidence, and blocker.
  - Field matrix and examples in [`conventions.md` § Task fields](../target-roadmap/conventions.md#task-fields); deferred/cancelled → appendix sections; Owner optional per task.
- [x] `JR-TARGET-TODO-004`: Define how README changes create or update follow-up todo items.
  - [`docs/target-roadmap/readme-sync.md`](../target-roadmap/readme-sync.md); scaffolding map cross-links.
- [x] `JR-TARGET-TODO-005`: Define when todo completion is sufficient for Jarvis handoff.
  - [`docs/target-roadmap/handoff.md`](../target-roadmap/handoff.md); three layers (README, required `PROJ-*`, user sign-off).

## Universal scaffolding

- [x] `JR-UNIVERSAL-001`: Create a generic ADR directory structure, index, template, and governance model.
  - [`docs/universal-adr/README.md`](../universal-adr/README.md); copy templates in [`docs/templates/universal-adr/`](../templates/universal-adr/).
- [x] `JR-UNIVERSAL-002`: Create README governance guidance that target projects can own independently.
  - [`docs/universal-readme/README.md`](../universal-readme/README.md); templates in [`docs/templates/universal-readme/`](../templates/universal-readme/).
- [x] `JR-UNIVERSAL-003`: Create documentation conventions for production source, tests, architecture docs, and user-facing docs.
  - [`docs/universal-docs/README.md`](../universal-docs/README.md); templates in [`docs/templates/universal-docs/`](../templates/universal-docs/).
- [x] `JR-UNIVERSAL-004`: Create a generic Cursor rules layout and rules index.
  - [`docs/universal-rules/README.md`](../universal-rules/README.md); templates in [`docs/templates/universal-rules/`](../templates/universal-rules/).
- [x] `JR-UNIVERSAL-005`: Create PR and commit communication guidance.
  - [`docs/universal-pr-commit/README.md`](../universal-pr-commit/README.md); templates in [`docs/templates/universal-pr-commit/`](../templates/universal-pr-commit/).
- [x] `JR-UNIVERSAL-006`: Create validation checklist templates with generic rows and stack-specific extension points.
  - [`docs/universal-validation/README.md`](../universal-validation/README.md); templates in [`docs/templates/universal-validation/`](../templates/universal-validation/).
- [x] `JR-UNIVERSAL-007`: Create handoff guidance that confirms no target-project reliance on Jarvis.
  - [`docs/universal-handoff/README.md`](../universal-handoff/README.md); template [`docs/templates/universal-handoff/handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md).

## Stack-specific scaffolding

- [x] `JR-STACK-001`: Define how Jarvis detects or asks for a target project's language and framework.
  - [`docs/stack-scaffolding/`](../stack-scaffolding/) (`detection.md`, `confirmation.md`); target template [`docs/templates/stack-scaffolding/stack-profile.example.md`](../templates/stack-scaffolding/stack-profile.example.md).
- [x] `JR-STACK-002`: Define how Jarvis selects stack-specific rules and best-practices docs.
  - [`docs/stack-scaffolding/selection.md`](../stack-scaffolding/selection.md), [`upstream-capabilities.md`](../stack-scaffolding/upstream-capabilities.md); templates `upstream-references.example.md`, `stack-framework-rule.example.mdc`.
- [x] `JR-STACK-003`: Define how Jarvis records package manager and validation commands without inventing commands.
  - [`docs/stack-scaffolding/commands.md`](../stack-scaffolding/commands.md); target template [`docs/templates/stack-scaffolding/commands.example.md`](../templates/stack-scaffolding/commands.example.md).
- [x] `JR-STACK-004`: Define testing strategy prompts for unit, integration, component, browser, and end-to-end layers.
  - [`docs/stack-scaffolding/testing.md`](../stack-scaffolding/testing.md); target template [`docs/templates/stack-scaffolding/testing-strategy.example.md`](../templates/stack-scaffolding/testing-strategy.example.md); target `PROJ-STACK-003`.
- [x] `JR-STACK-005`: Define runtime, deployment, secrets, and environment boundary prompts.
  - [`docs/stack-scaffolding/runtime.md`](../stack-scaffolding/runtime.md); target template [`docs/templates/stack-scaffolding/runtime-boundaries.example.md`](../templates/stack-scaffolding/runtime-boundaries.example.md); target `PROJ-STACK-004`.
- [x] `JR-STACK-006`: Define dependency and tooling review guidance for generated project docs.
  - [`docs/stack-scaffolding/dependencies.md`](../stack-scaffolding/dependencies.md); optional target `docs/stack/dependency-review.md`; target `PROJ-STACK-005`.
- [x] `JR-STACK-007`: Review legacy framework and library docs for possible generalized reuse.
  - [`docs/stack-scaffolding/legacy-review.md`](../stack-scaffolding/legacy-review.md); removed `frameworks/`, `libraries/`, `ai-agents/`, `mcp-servers/`, stack `.cursor/rules`; `source-registry.md` → [`upstream-capabilities.md`](../stack-scaffolding/upstream-capabilities.md).

## Agent and rule scaffolding

- [x] `JR-AGENT-001`: Generalize an agent roster inspired by WFD without importing WFD product or stack assumptions.
  - [`docs/universal-agents/README.md`](../universal-agents/README.md); template [`docs/templates/universal-agents/INDEX.example.md`](../templates/universal-agents/INDEX.example.md).
- [x] `JR-AGENT-002`: Create role contracts for Orchestrator, Planner, Builder, Tester, and Validator.
  - Templates: [`docs/templates/universal-agents/orchestrator.example.md`](../templates/universal-agents/orchestrator.example.md), [`planner.example.md`](../templates/universal-agents/planner.example.md), [`builder.example.md`](../templates/universal-agents/builder.example.md), [`tester.example.md`](../templates/universal-agents/tester.example.md), [`validator.example.md`](../templates/universal-agents/validator.example.md); pack README [`docs/universal-agents/README.md`](../universal-agents/README.md) § Copy and adapt (role contracts).
- [x] `JR-AGENT-003`: Define minimum read sets for each role to keep future sessions efficient.
  - [`docs/universal-agents/minimum-read-sets.md`](../universal-agents/minimum-read-sets.md); target paste [`docs/templates/universal-agents/minimum-read-sets.example.md`](../templates/universal-agents/minimum-read-sets.example.md); INDEX hook in [`INDEX.example.md`](../templates/universal-agents/INDEX.example.md).
- [x] `JR-AGENT-004`: Define handoff prompts that target projects can copy and own.
  - [`docs/universal-agents/handoff-prompts.md`](../universal-agents/handoff-prompts.md); target paste [`docs/templates/universal-agents/handoff-prompts.example.md`](../templates/universal-agents/handoff-prompts.example.md); orchestrator contract pointer only.
- [x] `JR-RULE-001`: Define always-apply versus topic-specific rule conventions.
  - [`docs/universal-rules/activation-modes.md`](../universal-rules/activation-modes.md); summary in universal-rules README.
- [x] `JR-RULE-002`: Define rule authoring guidance for target projects.
  - [`docs/universal-rules/authoring.md`](../universal-rules/authoring.md); target paste [`rules-authoring.md`](../templates/universal-rules/rules-authoring.md).
- [x] `JR-RULE-003`: Define how generated rules cite target-project ADRs and docs.
  - [`docs/universal-rules/adr-and-doc-citation.md`](../universal-rules/adr-and-doc-citation.md); stack template cites upstream via target `docs/stack/upstream-references.md`.
- [ ] `JR-RULE-004`: Revisit documentation conventions as Cursor rules (activation mode, globs, optional `.mdc`).
  - **Blocked:** reference material for doc-syntax enforcement not available yet. Until then: **doc-only** `docs/documentation-conventions.md` per `JR-RULE-001` / [`universal-docs`](../universal-docs/README.md).

## Orchestration model

- [x] `JR-ORCH-001`: Generalize WFD's task-folder model for target projects.
  - [`docs/orchestration/task-folder-model.md`](../orchestration/task-folder-model.md); templates [`docs/templates/orchestration/`](../templates/orchestration/); default `.cursor/orchestrations/{task-id}/`, `{project-slug}-NNN`.
- [x] `JR-ORCH-002`: Define a generic `task-manifest.json` shape.
  - [`docs/orchestration/task-manifest.md`](../orchestration/task-manifest.md); example [`task-manifest.example.json`](../templates/orchestration/_template/task-manifest.example.json) (`manifest_version` **1.0.0**, full WFD-shaped fields).
- [x] `JR-ORCH-003`: Define artifact ownership for plan, acceptance criteria, build log, test report, validation report, and human approval.
  - [`docs/orchestration/artifact-ownership.md`](../orchestration/artifact-ownership.md); stage templates in [`docs/templates/orchestration/_template/`](../templates/orchestration/_template/) (`plan.md`, `acceptance-criteria.md`, `test-matrix.md`, `build-log.md`, `test-report.md`, `validation-report.md`, `human-approval.md`).
- [x] `JR-ORCH-004`: Separate lifecycle gates from merge-ready or handoff checks.
  - [`docs/orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md); target template [`docs/templates/universal-rules/workflow-gates.mdc`](../templates/universal-rules/workflow-gates.mdc).
- [x] `JR-ORCH-005`: Define small, medium, and large initialization paths.
  - [`docs/orchestration/init-paths.md`](../orchestration/init-paths.md) — project init paths, run risk tiers, small paths A/B/C; cross-links in orchestration pack and scaffolding map.
- [x] `JR-ORCH-006`: Define loop behavior for failed validation or human rework.
  - [`docs/orchestration/loops-and-rework.md`](../orchestration/loops-and-rework.md); cross-links in orchestration pack README, artifact-ownership, task-manifest, agent templates.
- [x] `JR-ORCH-007`: Ensure copied orchestration artifacts are self-contained inside the target project.
  - [`docs/orchestration/self-containment.md`](../orchestration/self-containment.md); **ORCH-IND-01–10** in [`handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md); sanitized [`README.example.md`](../templates/orchestration/_template/README.example.md).

## Validation and handoff

- [x] `JR-VALIDATION-001`: Define generic validation categories for initialized projects.
  - [`docs/validation/categories.md`](../validation/categories.md) — **VAL-CAT-01**–**08**, **VAL-PHASE-INIT** / **VAL-PHASE-CHANGE**; index [`docs/validation/README.md`](../validation/README.md).
- [x] `JR-VALIDATION-002`: Define evidence expectations for documentation-only initialization.
  - [`docs/validation/doc-only-init-evidence.md`](../validation/doc-only-init-evidence.md) — **VAL-EVID-00**–**05**; doc-only definition; per-**VAL-CAT-*** minimums; **VAL-CAT-07** N/A phrase; `Evidence:` format.
- [x] `JR-VALIDATION-003`: Define evidence expectations for generated code, tooling, or config.
  - [`docs/validation/runnable-init-evidence.md`](../validation/runnable-init-evidence.md) — **VAL-EVID-06**–**07**; default quality chain; runnable slices; **VAL-CAT-07** pass/deferred/N/A lines.
- [ ] `JR-VALIDATION-004`: Define target-project self-containment checks.
  - Operational checklist: [`docs/universal-handoff/README.md`](../universal-handoff/README.md) (`JR-UNIVERSAL-007`); extend evidence tiers if needed.
- [ ] `JR-VALIDATION-005`: Define what Jarvis may claim as complete versus what requires human approval.

## Legacy repository review

- [ ] `JR-LEGACY-001`: Inventory current legacy files and mark each as keep, generalize, replace, or remove.
  - Partial: framework/library/MCP trees — see [`legacy-review.md`](../stack-scaffolding/legacy-review.md) § Inventory (`JR-STACK-007`); `standards/`, `tools/`, `architecture/`, `testing/`, `documentation/` still open.
- [ ] `JR-LEGACY-002`: Remove or fix broken references from legacy configuration files.
- [ ] `JR-LEGACY-003`: Decide whether `jarvis-config.json` remains useful under the new model.
  - Interim: deprecated stub (`JR-STACK-007`); remove file or replace with target-only template when closing task.
- [ ] `JR-LEGACY-004`: Decide whether `.cursor.json` remains useful under the new model.
  - Interim: deprecated router (`JR-STACK-007`); remove when closing task.
- [ ] `JR-LEGACY-005`: Move reviewed legacy material into the new documentation structure or delete it.
