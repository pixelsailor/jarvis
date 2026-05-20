# Jarvis platform specification

Stable reference for Jarvis platform behavior, terminology, and target-project expectations. For task checklists, see [`backlog.md`](./backlog.md). For unresolved choices, see [`open-decisions.md`](./open-decisions.md).

## Terminology

- `Jarvis repository`: The source repository containing Jarvis templates, roadmap material, rules, agent guidance, and orchestration patterns.
- `Target project`: The project Jarvis initializes or augments.
- `Target README`: The target project's root `README.md`, created or modified by Jarvis during initialization.
- `Project roadmap`: The target project's setup backlog under `docs/roadmap/`, separate from product docs and from Jarvis's own `docs/roadmap/` in this repository.
- `Universal scaffold`: Documentation and workflow material that can apply to most projects after project-specific adaptation.
- `Stack-specific scaffold`: Documentation and workflow material tailored to the target project's language, framework, runtime, deployment model, storage choices, testing tools, or agent tooling.
- `Handoff`: The point where the target project has enough self-contained documentation and workflow guidance to continue without Jarvis.

## Operating boundaries

- Jarvis may temporarily reside near a target project during setup, such as in the same workspace.
- Jarvis must not assume it will remain beside the target project indefinitely.
- Target projects must not depend on live links, relative references, imports, configuration extends, or continuing access to the Jarvis repository.
- Jarvis-generated files should be copied, adapted, and committed as target-project assets.
- Any references to Jarvis in generated output should explain provenance only when useful; they should not be required for normal project operation.

## Jarvis delivery model

- **Documentation-first:** Jarvis produces Markdown specs, ADRs, rules, agent contracts, todos, and validation guidance that the target project owns after handoff. Implementation code in the target repo is out of scope for Jarvis's core mission unless the user explicitly expands scope.
- **CLI and scripts:** A future CLI or generator scripts may automate copying or validating scaffolds; that is **out of scope** for the current platform buildout. Do not assume a Jarvis binary, npm package, or install step exists when initializing a target project.

## Target-project initialization flow

1. User asks Jarvis to initialize a new project or augment an existing one.
2. Jarvis identifies the target project path and confirms the intended scope.
3. Jarvis inspects the target project for an existing root README and other project-defining files.
4. If no target README exists, Jarvis asks for enough project specifics to draft a useful 10,000 foot overview.
5. If a target README exists, Jarvis treats it as source material, identifies missing high-level context, and proposes updates.
6. Jarvis creates or updates the target README first because it guides the rest of the scaffold.
7. Jarvis creates or updates the target project's `docs/roadmap/` backlog (see default layout below).
8. Jarvis scaffolds universal documentation and guardrails.
9. Jarvis identifies language and framework from repository files and user confirmation, then scaffolds stack-specific documentation based on the target README, stack profile, and verified manifests.
10. If the work is complex, Jarvis routes setup through an orchestration model with planned artifacts, test or validation evidence, and human approval.
11. Jarvis performs a handoff check to confirm the target project is self-contained.

## Target README responsibilities

The target README should:

- Explain what the project is and who it is for.
- Summarize the product or system at 10,000 feet.
- Name the intended tech stack at a high level.
- State durable boundaries that affect future implementation decisions.
- Route detailed design, architecture, setup, and workflow content to internal docs.
- Give human contributors and agents enough context to find or create the right ADRs, rules, best practices, and workflow files.
- Avoid becoming a dumping ground for framework playbooks or low-level commands.
- Stand alone after initialization without requiring readers or agents to consult Jarvis.

**Workflow (canonical outline, intake, audit, scaffolding map, handoff):** [`../target-readme/README.md`](../target-readme/README.md).

## Stack identification

Before stack-specific rules and upstream docs are authored in the target project, Jarvis:

1. Runs an evidence-based **detection pass** on the target repository (and named package root in monorepos).
2. Compares results to README § Technology Stack when present.
3. Presents a **single confirmation batch** for gaps, medium-confidence assumptions, and conflicts — not field-by-field re-asks when lockfiles are clear.
4. Records confirmed facts in target `docs/stack/stack-profile.md` and aligns README § Technology Stack.
5. **Selects** stack-specific rules and docs by **composing** confirmed capabilities (framework, libraries, language) via [`upstream-capabilities.md`](../stack-scaffolding/upstream-capabilities.md) and [`selection.md`](../stack-scaffolding/selection.md) — not opaque stack profile IDs; Jarvis does not host framework playbooks ([`legacy-review.md`](../stack-scaffolding/legacy-review.md)).
6. **Records** package manager and validation commands from manifests into README § Development and `docs/stack/commands.md` — never invented scripts ([`commands.md`](../stack-scaffolding/commands.md)).
7. **Maps** test layers to verified commands in `docs/stack/testing-strategy.md` when tooling exists ([`testing.md`](../stack-scaffolding/testing.md)).
8. **Documents** runtime, deployment, and secrets/env boundaries (names only) in `docs/stack/runtime-boundaries.md` when applicable ([`runtime.md`](../stack-scaffolding/runtime.md)).
9. **Reviews** dependencies and tooling alignment read-only — no automatic upgrades ([`dependencies.md`](../stack-scaffolding/dependencies.md)).

**Workflow:** [`../stack-scaffolding/README.md`](../stack-scaffolding/README.md) (detect → confirm → [commands](../stack-scaffolding/commands.md) → [testing](../stack-scaffolding/testing.md) → [runtime](../stack-scaffolding/runtime.md) → [dependencies](../stack-scaffolding/dependencies.md) → [select rules/docs](../stack-scaffolding/selection.md)). **Do not** treat co-located reference repos (e.g. WFD) as automatic stack evidence; stack idioms live in target `docs/stack/upstream-references.md`.

## Target project roadmap requirements

The project roadmap (setup backlog) should:

- Live under `docs/roadmap/` in the target project so planning tasks are not mixed with conventions, ADRs, or user-facing docs elsewhere in `docs/`.
- Use stable task IDs so work can resume across sessions.
- Separate universal baseline tasks from stack-specific tasks.
- Record dependencies, blockers, and completion evidence.
- Identify which tasks are required for handoff.
- Update when target README changes create follow-up documentation, ADR, rule, agent, or validation work.
- Avoid depending on chat history or Jarvis-local plan files.

### Default location and layout

| File | Role |
| --- | --- |
| `docs/roadmap/README.md` | Short index: purpose, handoff status, pointer to `backlog.md` |
| `docs/roadmap/backlog.md` | Checkbox tasks with stable `PROJ-*` IDs |

**Format:** Markdown checklists in `backlog.md`; one task per line with ID in backticks at the start. Full schema, field rules, and deferred/cancelled sections: [`../target-roadmap/conventions.md`](../target-roadmap/conventions.md). Workflow index: [`../target-roadmap/README.md`](../target-roadmap/README.md).

**Root README link:** Target README should route agents to `docs/roadmap/README.md` under Documentation while setup is in progress; trim or archive after handoff if the backlog is complete.

**Why `docs/roadmap/`:** Separates resumable setup planning from project documentation (guides, conventions, architecture writeups). Mirrors the Jarvis platform layout pattern without requiring a live link to Jarvis.

**When to use a different layout:** Honor an existing `docs/roadmap/` if the project already uses it for this purpose; merge tasks instead of creating a parallel backlog.

**When to add another file under `docs/roadmap/`:** Only for a clearly separate concern (for example `audit-backlog.md` after the main setup list is handed off), not as a second default artifact.

**Examples:** [`../../templates/target-project-roadmap/`](../../templates/target-project-roadmap/) in the Jarvis repository (`README.example.md`, `backlog.example.md`). Copy and adapt into the target project; not required at runtime.

### Task ID prefix (`PROJ-*`)

Use **`PROJ-{AREA}-{number}`** (for example `PROJ-ADR-001`).

| Prefix | Rationale |
| --- | --- |
| **`PROJ-*` (chosen)** | Reads naturally inside the target repository ("project roadmap task"). Area segment (`PROJ-ADR`, `PROJ-RULE`) stays stable across sessions. |
| **`TGT-*` (not used)** | "Target" is Jarvis-centric vocabulary for the repo being initialized; confusing once work lives inside that repo. |
| **`INIT-*` (not used)** | "Initial" is implied by setup phase but weakens after handoff if the backlog keeps tracking foundation work; `PROJ-*` fits ongoing project roadmap items. |

Do not use project-specific abbreviation prefixes (for example `WFD-*` in a non-WFD project); keep `PROJ-*` generic.

Suggested `PROJ-*` area segments:

- `PROJ-README-*`: Target README creation or revision.
- `PROJ-ADR-*`: ADR index, governance, templates, and project-specific decisions.
- `PROJ-RULE-*`: Cursor rules or equivalent agent guidance.
- `PROJ-DOC-*`: Documentation conventions and topic docs.
- `PROJ-AGENT-*`: Agent contracts, handoffs, and role guidance.
- `PROJ-ORCH-*`: Orchestration task folders, manifests, and artifacts.
- `PROJ-STACK-*`: Stack-specific setup, validation, and best practices.
- `PROJ-HANDOFF-*`: Self-containment and independence checks.

## WFD concepts to generalize

WFD should guide Jarvis's documentation and orchestration discipline, especially:

- Task-folder state for complex work.
- Manifest-owned resumability.
- Agent roles with explicit artifact ownership.
- Lifecycle gates separate from merge-ready or handoff checks.
- Acceptance criteria, build logs, test reports, validation reports, and human approval records.
- Fresh-session handoffs that avoid reliance on chat memory.
- Right-sized workflows for small, medium, and large changes.

**Canonical Jarvis docs (generalized, not WFD copies):** [`../orchestration/README.md`](../orchestration/README.md) — task folders under `.cursor/orchestrations/{task-id}/`, `{project-slug}-NNN` IDs, `task-manifest.json` schema (`JR-ORCH-001`, `JR-ORCH-002`); lifecycle vs merge-ready vs handoff — [`gates-and-checks.md`](../orchestration/gates-and-checks.md) (`JR-ORCH-004`); init and run-sizing paths — [`init-paths.md`](../orchestration/init-paths.md) (`JR-ORCH-005`); validation remediation and human rework loops — [`loops-and-rework.md`](../orchestration/loops-and-rework.md) (`JR-ORCH-006`); orchestration self-containment on copy — [`self-containment.md`](../orchestration/self-containment.md) (`JR-ORCH-007`). **Cursor rules:** activation modes, authoring, ADR/doc citation — [`../universal-rules/`](../universal-rules/README.md) (`JR-RULE-001`–`003`).

Do not copy WFD-specific details into Jarvis defaults, including:

- Product domain language.
- Framework choices.
- Storage choices.
- Cloud provider assumptions.
- AI provider assumptions.
- Package scripts.
- ADR or gap identifiers.
- Validation rows tied to WFD's product architecture.

## Handoff checklist for future Jarvis output

**Detailed criteria:** [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) (required `PROJ-*` vs optional rows, partial handoff, evidence). **README sync after edits:** [`../target-roadmap/readme-sync.md`](../target-roadmap/readme-sync.md). **Repo-wide independence audit:** [`../universal-handoff/README.md`](../universal-handoff/README.md) → target `docs/handoff-self-containment.md`.

Before considering a target project initialized, Jarvis should confirm:

- The target README stands alone.
- The target project has a durable `docs/roadmap/` backlog or equivalent completion record.
- Generated ADRs, rules, agents, and docs live inside the target project.
- Target `docs/handoff-self-containment.md` (or equivalent) passes — no required links to Jarvis; rules and agents cite target paths only; when orchestration exists, **ORCH-IND-01–10** pass per [`self-containment.md`](../orchestration/self-containment.md).
- Generated docs do not require relative links to Jarvis.
- Generated rules and agents cite target-project files, not Jarvis-local files.
- Stack-specific guidance matches the target project's actual tools.
- Known incomplete setup work is recorded in the target project.
- The user understands which remaining tasks are required before normal development begins.
