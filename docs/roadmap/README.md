# Jarvis platform roadmap

Entry point for Jarvis platform buildout planning. Read this file first in a cold session before diving into legacy framework or rule trees.

## Current status

Jarvis is being repurposed from a framework-oriented knowledge base into a project-foundation assistant. Existing repository content may be legacy until reviewed, generalized, or removed. The intended platform behavior is documentation-first: durable foundations before implementation, with target projects independent after handoff.

## Where to read

| Document | Use when |
| --- | --- |
| [`platform-spec.md`](./platform-spec.md) | You need terminology, boundaries, initialization flow, target README/todo requirements, WFD generalization notes, or the target handoff checklist. Content changes rarely. |
| [`backlog.md`](./backlog.md) | You are picking up, completing, or adding platform tasks (`JR-*` IDs). Content changes often. |
| [`open-decisions.md`](./open-decisions.md) | You need unresolved product or structure choices, or you are closing a decision. |
| [`MAINTENANCE.md`](./MAINTENANCE.md) | You are about to edit any roadmap file; read the update rules first. |
| [`../target-readme/README.md`](../target-readme/README.md) | You are creating, auditing, or handing off a **target** project's root README. |
| [`../target-roadmap/README.md`](../target-roadmap/README.md) | Target `PROJ-*` backlog: conventions, README sync, or handoff criteria. |
| [`../templates/target-project-roadmap/`](../templates/target-project-roadmap/) | You are scaffolding a target project's `docs/roadmap/` (`PROJ-*` backlog). |
| [`../universal-adr/README.md`](../universal-adr/README.md) | You are scaffolding or adapting target `adrs/` (index, governance, template). |
| [`../universal-readme/README.md`](../universal-readme/README.md) | You are scaffolding target README governance (`docs/readme-governance.md`, optional rule). |
| [`../universal-docs/README.md`](../universal-docs/README.md) | You are scaffolding target documentation conventions (`docs/documentation-conventions.md`, `docs/architecture/`, `docs/guides/`). |
| [`../universal-pr-commit/README.md`](../universal-pr-commit/README.md) | You are scaffolding target PR/commit guidance (`docs/pr-and-commit-guide.md`, optional `.github/pull_request_template.md`). |
| [`../universal-validation/README.md`](../universal-validation/README.md) | You are scaffolding target validation checklist (`docs/validation-checklist.md`, optional `validation-checklist.mdc`). |
| [`../universal-handoff/README.md`](../universal-handoff/README.md) | You are scaffolding target handoff self-containment (`docs/handoff-self-containment.md`) for `PROJ-HANDOFF-*`. |
| [`../stack-scaffolding/README.md`](../stack-scaffolding/README.md) | Target stack: detect/confirm (`JR-STACK-001`), then select rules and docs (`JR-STACK-002`). |

## Roadmap structure decision (`JR-TODO-004`)

**Decision:** Use a structured `docs/roadmap/` area instead of one monolithic todo file.

**Rationale (agent efficiency, clarity, readability):**

- **Targeted reads:** Stable platform spec (~150 lines) separates from the growing task backlog (~90 lines today, much larger as `JR-*` work lands). Agents resuming task work should not reload full spec every time.
- **Safer diffs:** Backlog-only edits stop touching terminology, boundaries, and handoff invariants.
- **Single front door:** This `README.md` stays the one link from the root README; no extra navigation depth beyond one hop.
- **Backward compatibility:** [`../jarvis-platform-todo.md`](../jarvis-platform-todo.md) remains a short redirect so old links and plans still resolve.

**Not chosen:** Keeping everything in `jarvis-platform-todo.md` — simpler today, but the backlog will outgrow comfortable single-file scanning and mixes volatile checklists with durable reference text.

**Revisit when:** The backlog exceeds ~400 lines or a second active workstream needs its own backlog file; then split `backlog.md` by theme (for example `backlog-scaffolding.md`) and update this index only.
