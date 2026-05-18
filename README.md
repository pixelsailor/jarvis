# Jarvis

Jarvis is being repurposed as a project-foundation assistant for standing up new or existing software projects with clear documentation, durable decisions, agent-friendly workflows, and consistent guardrails from the start.

The long-term goal is for Jarvis to help initialize a target project by drafting or updating its root README, creating a resumable `docs/roadmap/` backlog (`PROJ-*` tasks), and scaffolding the ADRs, rules, best practices, agent contracts, orchestration files, and structured documentation that the target project needs for its own stack.

## Refactor Notice

This repository is in transition. Much of the existing content was created for an earlier version of Jarvis that acted as a framework-oriented knowledge base for AI-assisted development. Those files may be incomplete, stale, broken, or too specific for the new direction.

Until each area is reviewed and intentionally ported, do not treat existing framework, library, rule, agent, or configuration files as authoritative. Prefer this README and the current roadmap documents when determining Jarvis's purpose.

What's For Dinner (WFD) is the conceptual template for the documentation and orchestration discipline Jarvis should eventually provide. Use WFD's patterns as inspiration for project scaffolding, handoffs, validation, and agent workflows. Do not copy WFD-specific product, framework, storage, provider, or package-script decisions into Jarvis defaults.

## Project Summary

Jarvis is intended to be used during project initialization or augmentation. A user can place Jarvis near a target project during setup, ask Jarvis to initialize or improve that project, and let Jarvis produce the target project's own self-contained documentation and workflow foundation.

Jarvis should not remain an operational dependency after setup. The target project must be able to live as an independent repository with its own README, todos, ADRs, rules, agents, workflows, and documentation. Any material generated from Jarvis should be copied, adapted, and owned by the target project.

## Core Principles

- Start with the project story. The target project's root README should establish the 10,000 foot purpose, audience, stack, and boundaries before deeper docs are generated.
- Support humans and agents equally. Documentation should be readable in review and precise enough to guide future agent sessions.
- Combine universal foundations with stack-specific guidance. Common ADR, workflow, and documentation patterns should be adapted to each target project's actual technology choices.
- Preserve durable decisions. Architecture, governance, and workflow choices should live in ADRs or focused internal docs rather than in chat history.
- Make initialization resumable. Long setup work should produce a durable todo list and, when needed, orchestration artifacts that survive context loss.
- Keep ownership clear. Jarvis provides patterns and scaffolding; the target project owns the resulting files after handoff.
- Avoid hidden dependencies. A completed target project should not require live links, relative references, or continued access to the Jarvis repository.

## Core Capabilities

Jarvis is being shaped around these capabilities:

- Initialize a new project foundation from a user prompt.
- Augment an existing project by reading its current README and documentation.
- Create or revise a target project's root README according to common README principles.
- Ask for project specifics when the target project lacks enough context for a useful README.
- Create and maintain a project roadmap under `docs/roadmap/` so setup can stop and resume safely.
- Use the target README to determine which universal and stack-specific docs are required.
- Scaffold ADRs, rules, best practices, agent contracts, orchestration files, validation guidance, and supporting documentation.
- Support a future orchestration model for complex initialization work that needs planning, implementation, testing, validation, and human approval.

These capabilities describe the intended direction. Jarvis is documentation-first today: it does not ship a CLI or generator scripts in this repository. A future CLI remains possible but out of scope for the current buildout.

## Documentation

The root README should stay high-level. Detailed guidance belongs in internal documents that can evolve without turning the README into an implementation manual.

Current and future documentation families include:

- Roadmaps and todos for the Jarvis platform buildout.
- ADR templates and governance guidance ([`docs/universal-adr/README.md`](./docs/universal-adr/README.md)).
- README governance for target projects ([`docs/universal-readme/README.md`](./docs/universal-readme/README.md)).
- Documentation conventions ([`docs/universal-docs/README.md`](./docs/universal-docs/README.md)).
- Cursor rules and rule indexes.
- Agent contracts and handoff prompts.
- Orchestration guides and task-folder templates.
- Universal scaffolding templates.
- Stack-specific guidance for frameworks, runtimes, testing, deployment, secrets, and tooling.

The active platform roadmap begins in [`docs/roadmap/README.md`](./docs/roadmap/README.md) (task backlog, platform spec, and maintenance rules). Target-project README workflow lives in [`docs/target-readme/README.md`](./docs/target-readme/README.md). [`docs/jarvis-platform-todo.md`](./docs/jarvis-platform-todo.md) redirects there for older links.

## Development

Jarvis itself is being rebuilt. During the transition, development guidance should be kept in focused internal docs and updated as the repository structure becomes intentional.

Contributors and agents should keep root README changes limited to durable purpose, principles, high-level capabilities, and documentation routing. Implementation playbooks, stack details, generated templates, and orchestration mechanics should live in dedicated docs.

Because the current repository content includes legacy material, avoid expanding or depending on old file catalogs without first reviewing whether they still serve the repurposed platform.

## Workflow

The intended Jarvis-assisted initialization flow is:

1. A user asks Jarvis to initialize a new project or augment an existing one.
2. Jarvis inspects the target project and determines whether a root README already exists.
3. If no target README exists, Jarvis asks for the project specifics needed to draft one.
4. If a target README exists, Jarvis analyzes it and proposes updates that clarify the project's purpose, stack, boundaries, and documentation needs.
5. Jarvis creates or updates the target project's `docs/roadmap/` backlog with stable `PROJ-*` tasks.
6. Jarvis scaffolds universal documentation and guardrails.
7. Jarvis adds stack-specific ADRs, rules, best practices, agent guidance, validation expectations, and workflow docs based on the target README.
8. For complex setup work, Jarvis uses or creates an orchestration process inspired by WFD's task-folder and gated handoff model.
9. Jarvis completes handoff by ensuring the target project is self-contained and does not rely on the Jarvis repository.

The target project's README is the starting point for project-specific scaffolding. Jarvis's README explains the platform; the target README explains the project being initialized.
