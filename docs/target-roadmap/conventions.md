# Target project roadmap — conventions and task fields

Markdown checklist conventions for `docs/roadmap/backlog.md` in a target project. Optimized for **cold-session agent resume**, **stable IDs**, and **minimal noise** in diffs.

**Scope:** Setup and foundation backlog (`PROJ-*`). Product feature roadmaps belong elsewhere (see target README § Roadmap direction).

**Decisions (human-confirmed for Jarvis defaults):**

| Topic | Choice |
| --- | --- |
| Deferred / cancelled tasks | Move rows to **`## Deferred`** or **`## Cancelled`** at the bottom of `backlog.md`; active sections list only open work |
| Owner | **Optional** on any task (`Owner: human` or `Owner: agent`) when responsibility would otherwise be ambiguous |

---

## File layout

| File | Required content |
| --- | --- |
| `docs/roadmap/README.md` | Purpose, handoff status summary, pointer to `backlog.md`, note that IDs are `PROJ-*` |
| `docs/roadmap/backlog.md` | Checkbox tasks grouped by area; optional **Last updated** date at top |

**Do not** duplicate the full backlog in the root README or in `platform-spec`-style prose elsewhere.

### `backlog.md` section order

1. **Title** — `# Project roadmap backlog` (or project name variant).
2. **Metadata** (optional) — `**Last updated:** YYYY-MM-DD` and one line pointing to `README.md`.
3. **Active area sections** — `## PROJ-{AREA} — {Title}` with open and recently completed tasks agents still need for context.
4. **`## How agents use this file`** — Short numbered list (see [template](../../templates/target-project-roadmap/backlog.example.md)).
5. **`## Deferred`** — Tasks paused with a known resume condition (may be empty).
6. **`## Cancelled`** — Tasks explicitly dropped with reason (may be empty).

Keep **active** sections free of deferred/cancelled rows so the next agent can scan open work without filtering.

---

## Task line schema (JR-TARGET-TODO-002)

One task per line. Use GitHub-flavored Markdown task list syntax.

### Canonical open task

```markdown
- [ ] `PROJ-ADR-001`: Add `adrs/` with index, governance, and template. **required for handoff**
  - Depends on: `PROJ-README-001`
  - Owner: agent
```

### Canonical completed task

```markdown
- [x] `PROJ-STACK-001`: Document package manager and validation commands from project files.
  - Evidence: `package.json` and `pnpm-lock.yaml` inspected 2026-05-17; commands copied to README § Development.
```

### Title rules

- **Imperative outcome** — what exists when done, not which files to touch (`Add ADR index`, not `Edit INDEX.md`).
- **ID first** — backtick-wrapped `PROJ-{AREA}-{number}` immediately after the checkbox.
- **Colon** — single colon separates ID from title.
- **Handoff marker** — append `**required for handoff**` only for tasks that block Jarvis/project handoff (see [Handoff marker](#handoff-marker)).
- **Optional scope note** — *(optional for small projects)* in the title when the row may be skipped on the small init path.

### Area sections

- **Heading:** `## PROJ-{AREA} — {Human title}` (em dash between code and title).
- **AREA** must match the ID segment (`PROJ-ADR-001` lives under `## PROJ-ADR — Architecture decisions`).
- **Order:** Follow scaffolding order when possible (README → roadmap → ADR → rules → doc → stack → agent → orch → handoff). See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md#scaffolding-order-default).

### ID rules (`PROJ-*`)

| Rule | Detail |
| --- | --- |
| Format | `PROJ-{AREA}-{NNN}` — three-digit zero-padded number per area (`PROJ-RULE-001`) |
| Stability | Never reuse an ID for a different meaning; retire via **Cancelled** if dropped |
| Prefix | Always `PROJ-*` in the target repo — not `JR-*`, not product ticket IDs |
| New areas | Add a new `## PROJ-{AREA}` section and document the area in a one-line comment in README if non-obvious |

Suggested areas: `README`, `ADR`, `RULE`, `DOC`, `AGENT`, `ORCH`, `STACK`, `HANDOFF` (see [`platform-spec.md`](../roadmap/platform-spec.md#task-id-prefix-proj)).

---

## Task fields (JR-TARGET-TODO-003)

| Field | Required | Where | Purpose |
| --- | --- | --- | --- |
| **ID** | Yes | Task line | Stable reference across sessions, ADRs, and rules |
| **Status** | Yes | Checkbox | `[ ]` open, `[x]` done |
| **Title** | Yes | Task line | Outcome statement |
| **Dependency** | When blocked by another task | Sub-bullet | Ordering: cannot start until cited `PROJ-*` is `[x]` |
| **Owner** | Optional | Sub-bullet | `Owner: human` or `Owner: agent` when responsibility is unclear |
| **Evidence** | When completing handoff-required tasks; otherwise when non-obvious | Sub-bullet | What was verified, run, or delivered |
| **Blocker** | When work cannot proceed | Sub-bullet | External or unresolved condition (not satisfied by finishing another `PROJ-*`) |
| **Handoff marker** | For handoff gates only | Task line | `**required for handoff**` |

### Status

| Checkbox | Meaning | Section |
| --- | --- | --- |
| `[ ]` | Open or in progress | Active area section |
| `[x]` | Complete | Active area section (keep until area is stable, then optional archive) |
| — | Paused, will resume | **`## Deferred`** — keep `[ ]`, add reason on sub-bullet |
| — | Will not do | **`## Cancelled`** — keep `[ ]` or use `[x]` with cancellation evidence; prefer `[ ]` + reason for clarity |

**Deferred row example:**

```markdown
## Deferred

- [ ] `PROJ-ORCH-001`: Add task-folder template for large setup.
  - Jarvis templates: [`../orchestration/`](../orchestration/), [`../templates/orchestration/_template/`](../templates/orchestration/_template/).
  - Owner: agent
```

**Cancelled row example:**

```markdown
## Cancelled

- [ ] `PROJ-AGENT-001`: Add full agent roster.
  - Cancelled: project chose small init path; user confirmed agents not needed at setup.
  - Owner: human
```

Do not leave deferred/cancelled tasks in active `## PROJ-*` sections.

### Dependency

- **Sub-bullet label:** `Depends on:` (singular list; multiple IDs comma-separated).
- **Required when:** Starting the task before the dependency is done would waste work or violate order (see scaffolding map).
- **Not a dependency:** Soft preference, "nice to have" ordering — omit the sub-bullet.
- **Verify before start:** All cited IDs are `[x]` in active or completed form (not in **Cancelled** unless the dependency was replaced).

```markdown
  - Depends on: `PROJ-README-001`, `PROJ-ADR-001`
```

### Owner

- **Values:** `Owner: human` | `Owner: agent` (lowercase role words; no email handles in the default schema).
- **Use when:** Handoff sign-off, product calls, access/credentials, or ambiguous split between user and agent.
- **Omit when:** Obvious agent setup work during Jarvis initialization.
- **Do not** use Owner to mean "assignee name" — this backlog is for agents and contributors, not HR tracking.

### Evidence

- **Sub-bullet label:** `Evidence:` — one line preferred; wrap with semicolons if needed.
- **Required when:** Marking `[x]` on any task with **`required for handoff`**.
- **Optional otherwise:** Add only when verification steps matter for the next agent (commands run, files created, audit date).
- **Good:** Points to repo paths, dates, or commands actually run — not "done" or "completed".
- **Bad:** Restating the title; chat-only claims with no artifact.

```markdown
  - Evidence: `adrs/INDEX.md` created; `pnpm run check` passed 2026-05-17.
```

**Initialization audits** (when the project used category IDs during setup): prefer one line per category audited:

```markdown
  - Evidence: VAL-CAT-02 PASS — VAL-EVID-02/03: README § Documentation links verified 2026-05-27; paths: README.md, docs/roadmap/README.md
```

Use `N/A` with a one-line reason when a category does not apply (for example doc-only init with no runnable artifact: `VAL-CAT-07 N/A — doc-only init; no runnable artifact produced`). Do not cite Jarvis platform paths in evidence.

### Blocker

- **Sub-bullet label:** `Blocker:` — what prevents progress **now**.
- **Use when:** Dependencies are satisfied but work still cannot proceed (waiting on user, vendor, legal, upstream release).
- **Contrast with Depends on:** Dependency = another `PROJ-*` must finish first; Blocker = outside the backlog graph or unresolved decision.
- **Clear blocker:** Remove the sub-bullet in the same session when unblocked; do not mark `[x]` with an open blocker.

```markdown
  - Blocker: user has not confirmed cloud provider; needed before `PROJ-STACK-002`.
```

### Handoff marker

- **Syntax:** `**required for handoff**` at end of task line (bold phrase, exact wording).
- **Use for:** Tasks that must be complete before claiming target-project independence from Jarvis (see [`handoff.md`](./handoff.md) and `PROJ-HANDOFF-*`).
- **Do not** mark every task — only gates that define "initialized enough to develop."

---

## Field quick reference (agent scan)

```text
- [ ] `PROJ-XXX-NNN`: <imperative title>. **required for handoff**
  - Depends on: `PROJ-YYY-NNN`
  - Owner: human | agent
  - Blocker: <short reason>
  - Evidence: <only on completion, or when documenting verification>
```

---

## Agent edit rules

1. **Read** root README → `docs/roadmap/README.md` → `backlog.md` → this doc if editing conventions.
2. **Pick work** — Lowest-numbered open `PROJ-*` in the earliest active section whose dependencies are `[x]` and has no `Blocker:` sub-bullet.
3. **Complete** — Set `[x]`; add `Evidence:` when required; remove `Blocker:` when cleared.
4. **Spawn** — New tasks from README changes per [`scaffolding-map.md`](../target-readme/scaffolding-map.md#when-readme-changes--update-backlog); never reuse IDs.
5. **Defer / cancel** — Move the full row (sub-bullets included) to **Deferred** or **Cancelled**; do not delete history without user direction.
6. **IDs in other files** — When an ADR or rule cites `PROJ-*`, update the backlog in the same session if scope changes.

---

## Existing roadmaps

If the target project already has `docs/roadmap/`:

- **Merge** new `PROJ-*` tasks into the existing `backlog.md` structure when possible.
- **Do not** create `backlog-jarvis.md` or a second ID scheme in parallel.
- **Honor** existing area names only when they already map cleanly; otherwise add standard `PROJ-*` sections and link from README.

Pause and ask the user when an existing backlog uses a non-markdown system (issue tracker export, YAML-only) and migration scope is unclear.

---

## Anti-patterns

| Anti-pattern | Why |
| --- | --- |
| Chat-only plan instead of backlog updates | Breaks resumability |
| Duplicate task in active + Deferred | Ambiguous status |
| `Depends on:` for every row | Noise; weakens real ordering |
| `[x]` without Evidence on **required for handoff** tasks | Handoff cannot be audited |
| Invented `PROJ-STACK-*` evidence | Commands must match repo files |
| `JR-*` IDs in target backlog | Wrong repository vocabulary |
| Tables as the primary task store | Harder to diff and complete in editor; use checklists |
| Long prose paragraphs under a task | Move design text to ADRs or `docs/` topic files |

---

## Maintenance (target project)

After handoff, the target team owns edits. Suggested discipline (mirror Jarvis [`MAINTENANCE.md`](../roadmap/MAINTENANCE.md)):

- Update **Last updated** when changing backlog structure or handoff status.
- One task per line; completion notes stay sub-bullets.
- Product feature work does not belong in this file — use the product roadmap doc linked from the README.

---

## Examples

Full file: [`../../templates/target-project-roadmap/backlog.example.md`](../../templates/target-project-roadmap/backlog.example.md).

**Minimal open task:**

```markdown
- [ ] `PROJ-DOC-001`: Add documentation conventions under `docs/`.
```

**Handoff gate with owner:**

```markdown
- [ ] `PROJ-HANDOFF-003`: User acknowledges remaining non-handoff tasks before feature development. **required for handoff**
  - Owner: human
```
