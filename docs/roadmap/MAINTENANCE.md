# Jarvis roadmap maintenance

Rules for humans and agents editing files under `docs/roadmap/`. Goal: resumable platform work without chat memory, minimal noise in diffs, and clear ownership of each file.

## Which file to edit

| Change | Edit |
| --- | --- |
| New, completed, deferred, or removed platform task (`JR-*`) | [`backlog.md`](./backlog.md) only |
| Terminology, boundaries, initialization flow, target README/todo requirements, WFD generalization, handoff checklist | [`platform-spec.md`](./platform-spec.md) |
| Target README outline, intake, audit, scaffolding map, README handoff checklist | [`../target-readme/`](../target-readme/) (not `platform-spec.md`) |
| Target `PROJ-*` backlog schema, task fields, deferred/cancelled conventions | [`../target-roadmap/conventions.md`](../target-roadmap/conventions.md) |
| README → backlog sync; Jarvis handoff sufficiency | [`../target-roadmap/readme-sync.md`](../target-roadmap/readme-sync.md), [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) |
| Unresolved product or structure question; recording a closed decision | [`open-decisions.md`](./open-decisions.md) |
| New roadmap file, link targets, or structure decision | [`README.md`](./README.md) and root [`README.md`](../../README.md) if the public entry point changes |
| When agents must update the roadmap (this document) | [`MAINTENANCE.md`](./MAINTENANCE.md) |
| Universal ADR scaffold (copy/adapt for targets) | [`../universal-adr/README.md`](../universal-adr/README.md) |
| Universal README governance scaffold (copy/adapt for targets) | [`../universal-readme/README.md`](../universal-readme/README.md) |
| Universal documentation conventions scaffold (copy/adapt for targets) | [`../universal-docs/README.md`](../universal-docs/README.md) |
| Universal PR/commit communication scaffold (copy/adapt for targets) | [`../universal-pr-commit/README.md`](../universal-pr-commit/README.md) |
| Universal validation checklist scaffold (copy/adapt for targets) | [`../universal-validation/README.md`](../universal-validation/README.md) |
| Universal handoff self-containment scaffold (copy/adapt for targets) | [`../universal-handoff/README.md`](../universal-handoff/README.md) |
| Stack detection, confirmation, commands, testing, runtime, dependencies, and rule/doc selection | [`../stack-scaffolding/README.md`](../stack-scaffolding/README.md) |
| Universal orchestration task-folder model, manifest schema, and artifact ownership | [`../orchestration/README.md`](../orchestration/README.md) |
| Universal agent roster and role contract scaffolds | [`../universal-agents/README.md`](../universal-agents/README.md) |

Do not duplicate backlog tasks in `platform-spec.md` or prose summaries of the backlog in the root README.

## When agents must update the roadmap

Update in the **same session** as the work when any of the following apply:

1. **Task lifecycle:** You start, finish, defer, split, or cancel a `JR-*` item — toggle its checkbox and add a one-line completion note under the task only if evidence or follow-up is non-obvious.
2. **New platform work discovered:** You identify work that belongs on the Jarvis platform (not a target project) — add a new `JR-*` row in the correct backlog section with a stable ID; do not invent IDs outside the section prefix pattern already in use.
3. **Spec drift:** Your change alters platform behavior, boundaries, or initialization flow described in `platform-spec.md` — update the spec in the same change set.
4. **Decision closure:** A row in `open-decisions.md` is resolved — move the outcome into `platform-spec.md` or `README.md`, then remove or strike the open row and note the date in the closure (inline in `open-decisions.md` history is optional; prefer spec as source of truth).
5. **Structural change:** Files are added, removed, or renamed under `docs/roadmap/` — update `README.md`, [`../jarvis-platform-todo.md`](../jarvis-platform-todo.md) if it is still the redirect stub, and root `README.md` links.

## When agents should not update the roadmap

- Target-project roadmaps (`docs/roadmap/` with `PROJ-*` IDs) belong in the **target project**, not here.
- Chat-only plans (for example `.cursor/plans/`) are not substitutes for backlog updates.
- Do not mark `JR-*` complete without doing the work or without explicit human direction to defer or cancel.
- Do not expand `platform-spec.md` with stack-specific defaults, WFD product details, or target-project ADR content.

## Task ID and checklist conventions

- **Prefix:** Keep section prefixes (`JR-README-*`, `JR-TODO-*`, `JR-TARGET-README-*`, etc.) aligned with backlog section titles.
- **New IDs:** Use the next free number in that section; never reuse a retired ID for a different meaning.
- **Checkboxes:** `- [ ]` open, `- [x]` complete. One task per line; task ID in backticks at the start of the line.
- **Scope:** Task title is imperative and names an outcome, not a file list.

## Completion evidence

For completed tasks, inline notes are optional. Add a short sub-bullet only when:

- Verification steps matter for the next agent (commands run, files created), or
- Follow-up `JR-*` items were spawned.

Example:

```markdown
- [x] `JR-TODO-004`: Decide whether this roadmap remains a single file or moves into a structured `docs/roadmap/` area.
  - Decision recorded in `docs/roadmap/README.md`; split into spec, backlog, open-decisions, maintenance.
```

## Root README coordination

- Root [`README.md`](../../README.md) stays high-level: purpose, principles, capabilities, workflow summary, link into `docs/roadmap/README.md`.
- After substantive platform-spec changes, check whether root README routing or capability bullets need a one-line adjustment; avoid copying the full spec into the root README.

## Human input

Pause and ask the user before:

- Reprioritizing or deleting large backlog sections,
- Changing the independence or documentation-first principles in `platform-spec.md`,
- Resolving `open-decisions.md` items that affect target-project defaults or mandatory scaffolds,
- Splitting `backlog.md` into multiple files (structural revisit per `README.md`).

Routine checkbox updates and spec edits that directly reflect completed, agreed work do not require extra approval.

## Redirect stub

[`../jarvis-platform-todo.md`](../jarvis-platform-todo.md) must remain a short pointer to `docs/roadmap/README.md` unless the project explicitly retires it. Do not grow the stub into a second full copy of the roadmap.
