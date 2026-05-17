# Jarvis Platform Todo

This file tracks the buildout of Jarvis as a project-foundation assistant. It is not a target project's initialization todo list.

Jarvis's root README is the public 10,000 foot charter. This file carries the operational detail future agents need to continue the repurpose across context sessions.

## Current Status

Jarvis is being repurposed from a framework-oriented knowledge base into a platform for initializing or augmenting independent software projects. Existing repository content may be legacy material until it is reviewed, generalized, or removed.

The intended platform behavior is documentation-first. Jarvis should help create durable project foundations before implementation work begins: README, todos, ADRs, rules, best practices, agent contracts, orchestration artifacts, validation guidance, and stack-specific documentation.

Jarvis should not become an ongoing dependency of initialized projects. Generated or adapted files must be owned by the target project after handoff.

## Terminology

- `Jarvis repository`: The source repository containing Jarvis templates, roadmap material, rules, agent guidance, and orchestration patterns.
- `Target project`: The project Jarvis initializes or augments.
- `Target README`: The target project's root `README.md`, created or modified by Jarvis during initialization.
- `Project initialization todo`: A durable todo file inside the target project that records remaining setup work.
- `Universal scaffold`: Documentation and workflow material that can apply to most projects after project-specific adaptation.
- `Stack-specific scaffold`: Documentation and workflow material tailored to the target project's language, framework, runtime, deployment model, storage choices, testing tools, or agent tooling.
- `Handoff`: The point where the target project has enough self-contained documentation and workflow guidance to continue without Jarvis.

## Operating Boundaries

- Jarvis may temporarily reside near a target project during setup, such as in the same workspace.
- Jarvis must not assume it will remain beside the target project indefinitely.
- Target projects must not depend on live links, relative references, imports, configuration extends, or continuing access to the Jarvis repository.
- Jarvis-generated files should be copied, adapted, and committed as target-project assets.
- Any references to Jarvis in generated output should explain provenance only when useful; they should not be required for normal project operation.

## Target-Project Initialization Flow

1. User asks Jarvis to initialize a new project or augment an existing one.
2. Jarvis identifies the target project path and confirms the intended scope.
3. Jarvis inspects the target project for an existing root README and other project-defining files.
4. If no target README exists, Jarvis asks for enough project specifics to draft a useful 10,000 foot overview.
5. If a target README exists, Jarvis treats it as source material, identifies missing high-level context, and proposes updates.
6. Jarvis creates or updates the target README first because it guides the rest of the scaffold.
7. Jarvis creates or updates a project initialization todo file inside the target project.
8. Jarvis scaffolds universal documentation and guardrails.
9. Jarvis scaffolds stack-specific documentation based on the target README, detected project files, and user answers.
10. If the work is complex, Jarvis routes setup through an orchestration model with planned artifacts, test or validation evidence, and human approval.
11. Jarvis performs a handoff check to confirm the target project is self-contained.

## Target README Responsibilities

The target README should:

- Explain what the project is and who it is for.
- Summarize the product or system at 10,000 feet.
- Name the intended tech stack at a high level.
- State durable boundaries that affect future implementation decisions.
- Route detailed design, architecture, setup, and workflow content to internal docs.
- Give human contributors and agents enough context to find or create the right ADRs, rules, best practices, and workflow files.
- Avoid becoming a dumping ground for framework playbooks or low-level commands.
- Stand alone after initialization without requiring readers or agents to consult Jarvis.

## Target Project Todo Requirements

The project initialization todo should:

- Live inside the target project.
- Use stable task IDs so work can resume across sessions.
- Separate universal baseline tasks from stack-specific tasks.
- Record dependencies, blockers, and completion evidence.
- Identify which tasks are required for handoff.
- Update when target README changes create follow-up documentation, ADR, rule, agent, or validation work.
- Avoid depending on chat history or Jarvis-local plan files.

Suggested target-project todo categories:

- `INIT-README-*`: Target README creation or revision.
- `INIT-ADR-*`: ADR index, governance, templates, and project-specific decisions.
- `INIT-RULE-*`: Cursor rules or equivalent agent guidance.
- `INIT-DOC-*`: Documentation conventions and topic docs.
- `INIT-AGENT-*`: Agent contracts, handoffs, and role guidance.
- `INIT-ORCH-*`: Orchestration task folders, manifests, and artifacts.
- `INIT-STACK-*`: Stack-specific setup, validation, and best practices.
- `INIT-HANDOFF-*`: Self-containment and independence checks.

## Jarvis Platform Backlog

### Root Positioning

- [x] `JR-README-001`: Rewrite Jarvis's root README around the new project-foundation mission.
- [x] `JR-README-002`: Add a refactor notice warning that old files may be stale or legacy.
- [x] `JR-README-003`: Clarify that WFD is a conceptual template, not stack-specific source material to copy.
- [x] `JR-README-004`: State that target projects must be independent after handoff.
- [ ] `JR-README-005`: Revisit the README after core docs exist and replace transition language with stable documentation links.

### Platform Todo And Roadmap

- [x] `JR-TODO-001`: Create a Jarvis-owned roadmap/todo document for the repurpose.
- [x] `JR-TODO-002`: Distinguish Jarvis's own README from target-project READMEs.
- [x] `JR-TODO-003`: Capture the target-project independence requirement.
- [ ] `JR-TODO-004`: Decide whether this roadmap remains a single file or moves into a structured `docs/roadmap/` area.
- [ ] `JR-TODO-005`: Add maintenance rules for when agents update this file.

### Target README Workflow

- [ ] `JR-TARGET-README-001`: Define the canonical target README outline Jarvis should generate.
- [ ] `JR-TARGET-README-002`: Define the minimum questions Jarvis asks when no target README exists.
- [ ] `JR-TARGET-README-003`: Define how Jarvis audits an existing target README before editing.
- [ ] `JR-TARGET-README-004`: Define how the target README drives downstream ADR, rule, doc, and agent scaffolding.
- [ ] `JR-TARGET-README-005`: Define a handoff checklist for target README self-containment.

### Target Project Todo Workflow

- [ ] `JR-TARGET-TODO-001`: Choose the default filename and location for target-project initialization todos.
- [ ] `JR-TARGET-TODO-002`: Define the todo schema or Markdown conventions.
- [ ] `JR-TARGET-TODO-003`: Define required fields such as ID, status, dependency, owner, evidence, and blocker.
- [ ] `JR-TARGET-TODO-004`: Define how README changes create or update follow-up todo items.
- [ ] `JR-TARGET-TODO-005`: Define when todo completion is sufficient for Jarvis handoff.

### Universal Scaffolding

- [ ] `JR-UNIVERSAL-001`: Create a generic ADR directory structure, index, template, and governance model.
- [ ] `JR-UNIVERSAL-002`: Create README governance guidance that target projects can own independently.
- [ ] `JR-UNIVERSAL-003`: Create documentation conventions for production source, tests, architecture docs, and user-facing docs.
- [ ] `JR-UNIVERSAL-004`: Create a generic Cursor rules layout and rules index.
- [ ] `JR-UNIVERSAL-005`: Create PR and commit communication guidance.
- [ ] `JR-UNIVERSAL-006`: Create validation checklist templates with generic rows and stack-specific extension points.
- [ ] `JR-UNIVERSAL-007`: Create handoff guidance that confirms no target-project reliance on Jarvis.

### Stack-Specific Scaffolding

- [ ] `JR-STACK-001`: Define how Jarvis detects or asks for a target project's language and framework.
- [ ] `JR-STACK-002`: Define how Jarvis selects stack-specific rules and best-practices docs.
- [ ] `JR-STACK-003`: Define how Jarvis records package manager and validation commands without inventing commands.
- [ ] `JR-STACK-004`: Define testing strategy prompts for unit, integration, component, browser, and end-to-end layers.
- [ ] `JR-STACK-005`: Define runtime, deployment, secrets, and environment boundary prompts.
- [ ] `JR-STACK-006`: Define dependency and tooling review guidance for generated project docs.
- [ ] `JR-STACK-007`: Review legacy framework and library docs for possible generalized reuse.

### Agent And Rule Scaffolding

- [ ] `JR-AGENT-001`: Generalize an agent roster inspired by WFD without importing WFD product or stack assumptions.
- [ ] `JR-AGENT-002`: Create role contracts for Orchestrator, Planner, Builder, Tester, and Validator.
- [ ] `JR-AGENT-003`: Define minimum read sets for each role to keep future sessions efficient.
- [ ] `JR-AGENT-004`: Define handoff prompts that target projects can copy and own.
- [ ] `JR-RULE-001`: Define always-apply versus topic-specific rule conventions.
- [ ] `JR-RULE-002`: Define rule authoring guidance for target projects.
- [ ] `JR-RULE-003`: Define how generated rules cite target-project ADRs and docs.

### Orchestration Model

- [ ] `JR-ORCH-001`: Generalize WFD's task-folder model for target projects.
- [ ] `JR-ORCH-002`: Define a generic `task-manifest.json` shape.
- [ ] `JR-ORCH-003`: Define artifact ownership for plan, acceptance criteria, build log, test report, validation report, and human approval.
- [ ] `JR-ORCH-004`: Separate lifecycle gates from merge-ready or handoff checks.
- [ ] `JR-ORCH-005`: Define small, medium, and large initialization paths.
- [ ] `JR-ORCH-006`: Define loop behavior for failed validation or human rework.
- [ ] `JR-ORCH-007`: Ensure copied orchestration artifacts are self-contained inside the target project.

### Validation And Handoff

- [ ] `JR-VALIDATION-001`: Define generic validation categories for initialized projects.
- [ ] `JR-VALIDATION-002`: Define evidence expectations for documentation-only initialization.
- [ ] `JR-VALIDATION-003`: Define evidence expectations for generated code, tooling, or config.
- [ ] `JR-VALIDATION-004`: Define target-project self-containment checks.
- [ ] `JR-VALIDATION-005`: Define what Jarvis may claim as complete versus what requires human approval.

### Legacy Repository Review

- [ ] `JR-LEGACY-001`: Inventory current legacy files and mark each as keep, generalize, replace, or remove.
- [ ] `JR-LEGACY-002`: Remove or fix broken references from legacy configuration files.
- [ ] `JR-LEGACY-003`: Decide whether `jarvis-config.json` remains useful under the new model.
- [ ] `JR-LEGACY-004`: Decide whether `.cursor.json` remains useful under the new model.
- [ ] `JR-LEGACY-005`: Move reviewed legacy material into the new documentation structure or delete it.

## WFD Concepts To Generalize

WFD should guide Jarvis's documentation and orchestration discipline, especially:

- Task-folder state for complex work.
- Manifest-owned resumability.
- Agent roles with explicit artifact ownership.
- Lifecycle gates separate from merge-ready or handoff checks.
- Acceptance criteria, build logs, test reports, validation reports, and human approval records.
- Fresh-session handoffs that avoid reliance on chat memory.
- Right-sized workflows for small, medium, and large changes.

Do not copy WFD-specific details into Jarvis defaults, including:

- Product domain language.
- Framework choices.
- Storage choices.
- Cloud provider assumptions.
- AI provider assumptions.
- Package scripts.
- ADR or gap identifiers.
- Validation rows tied to WFD's product architecture.

## Handoff Checklist For Future Jarvis Output

Before considering a target project initialized, Jarvis should confirm:

- The target README stands alone.
- The target project has a durable initialization todo or equivalent completion record.
- Generated ADRs, rules, agents, and docs live inside the target project.
- Generated docs do not require relative links to Jarvis.
- Generated rules and agents cite target-project files, not Jarvis-local files.
- Stack-specific guidance matches the target project's actual tools.
- Known incomplete setup work is recorded in the target project.
- The user understands which remaining tasks are required before normal development begins.

## Open Design Decisions

- Should Jarvis remain documentation-first, or eventually provide scripts or a CLI?
- What should the default target-project todo filename and format be?
- Which universal scaffolds are mandatory for every target project?
- Which scaffolds should be optional based on project size and risk?
- How much should Jarvis infer from repository files before asking the user?
- Should Jarvis maintain a catalog of stack profiles, or compose stack guidance from smaller capabilities?
- How should Jarvis verify generated files without depending on target-project tooling that may not exist yet?
