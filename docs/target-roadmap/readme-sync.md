# README changes → backlog updates

How Jarvis (or a follow-on agent) turns **target README** edits into durable `PROJ-*` rows in `docs/roadmap/backlog.md`. Complements [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) (initial README signals → areas) and [`conventions.md`](./conventions.md) (task line shape).

**Prerequisite:** `docs/roadmap/` exists with at least `README.md` and `backlog.md`.

---

## When this workflow runs

| Trigger | Action |
| --- | --- |
| Material README edit (see below) | Run full sync in the **same session** |
| New README draft from intake/audit | Seed backlog per scaffolding map, then sync |
| README handoff checklist failure | Fix README **or** backlog; do not mark `PROJ-README-*` done until aligned |
| Immaterial README edit | No new `PROJ-*` rows (optional `Last updated` on backlog only) |

### Material vs immaterial

**Material** — changes what agents or implementers should do, believe, or verify:

- Principles, non-negotiables, audience, product scope
- Core capabilities (add/remove/rename)
- Technology stack (add/remove/reorder components)
- Architecture boundaries, data ownership, security posture
- Documentation map links (new, removed, or retargeted paths)
- Development commands or environment variable names
- Roadmap direction that implies new foundation docs or ADRs
- Explicit "out of scope" / "deferred" statements that contradict open `PROJ-*` rows

**Immaterial** — no backlog sync required:

- Typos, formatting, badges, contributor names
- Clarifications that do not change behavior or artifact expectations
- Provenance wording that does not add Jarvis dependencies

When unsure, treat as **material** and add a lightweight `PROJ-README-*` audit note rather than skipping sync.

---

## Agent workflow (same session)

```text
1. Diff mentally: which README sections changed?
2. Classify: material or immaterial?
3. For each material change → map to PROJ-{AREA} (table below)
4. Update backlog.md (add | reopen | defer | cancel | stale sub-bullet)
5. Update docs/roadmap/README.md handoff summary if gates changed
6. Do not mark PROJ-README-* complete until README ↔ backlog align
```

**Rule:** Chat promises are invalid. Every new README obligation needs a `PROJ-*` row or README text must be removed.

---

## README section → backlog mapping

Use [`scaffolding-map.md` § Signal → backlog](../target-readme/scaffolding-map.md#signal--backlog--artifacts) for **initial** scaffolding. This table is for **incremental** README edits.

| README section / change | Backlog action | Typical IDs |
| --- | --- | --- |
| New or tightened principle / non-negotiable | Add `PROJ-ADR-*` to Accept; optional `PROJ-RULE-*` if enforceable in rules | Next free in area |
| Principle removed or weakened | Reopen or cancel ADR/rule tasks; add stale note on affected rows | Existing IDs |
| Capability added | `PROJ-DOC-*` and/or product doc task; ADR if architectural | New `PROJ-DOC-*` |
| Capability removed | Cancel open doc tasks; stale note on completed docs if misleading | |
| Stack component added | `PROJ-STACK-*` with **Evidence** from repo inspection; update `docs/stack/stack-profile.md` per [`stack-scaffolding/confirmation.md`](../stack-scaffolding/confirmation.md#re-verification-triggers) | New `PROJ-STACK-*` |
| Stack component removed | Cancel or defer stack tasks; update README § Development and `docs/stack/stack-profile.md` with evidence | |
| Boundary tightened (secrets, client/server, offline) | `PROJ-ADR-*` + `PROJ-RULE-*` | Often reopens work |
| Data ownership changed | `PROJ-ADR-*`; may reopen `PROJ-DOC-*` | |
| New Documentation map link | `PROJ-DOC-*` until path exists or link removed | |
| Link removed from map | Cancel `PROJ-DOC-*` if only for that path | |
| Development command added/changed | `PROJ-STACK-*`; verify command in repo before `[x]` | |
| Env var name added | `PROJ-STACK-*` or `PROJ-DOC-*` for env doc | |
| Roadmap direction (product themes) | Product doc under `docs/` — **not** `docs/roadmap/backlog.md` | `PROJ-DOC-*` |
| Init path upgrade (small → medium) | Add missing area sections and rows from scaffolding map | Bulk add |
| Handoff claimed in README prose | **Do not** close backlog by README alone — run [`handoff.md`](./handoff.md) | `PROJ-HANDOFF-*` |

---

## Backlog operations

### Add a new task

1. Choose area `PROJ-{AREA}` matching scaffolding map.
2. Assign next `NNN` in that area (never reuse retired IDs).
3. Write imperative title; add `Depends on:` when order matters.
4. Add `**required for handoff**` only if the change is a handoff gate (see [`handoff.md`](./handoff.md)).
5. Add `Owner:` when human vs agent is unclear.

```markdown
- [ ] `PROJ-ADR-003`: Accept ADR for offline-first client data authority.
  - Depends on: `PROJ-ADR-001`
  - Owner: human
```

### Reopen a completed task

Use when README **contradicts** a checked `[x]` artifact:

1. Change `[x]` → `[ ]` in the **active** section (move from archive if needed).
2. Add sub-bullet: `Stale: README now requires …; was completed before <date>.`
3. Do not delete prior `Evidence:` — append new line explaining reopen.

Pause and ask the user when reopening would invalidate an **Accepted** ADR without a recorded supersession plan.

### Defer or cancel

| Situation | Action |
| --- | --- |
| README defers work explicitly | Move row to **`## Deferred`** with `Deferred:` reason citing README |
| README drops scope | Move row to **`## Cancelled`** with `Cancelled:` reason; **Owner: human** if user directed |
| Optional path no longer applies | Cancel with small/medium/large path note |

Do not leave deferred/cancelled rows in active sections ([`conventions.md`](./conventions.md)).

### Stale downstream (artifact exists but README diverged)

When an ADR, rule, or doc exists but README no longer matches:

- Prefer **reopen** the producing `PROJ-*` task **or** add a new task with `Depends on:` the original.
- Add sub-bullet on related rows: `Stale: contradicts README § <section> as of YYYY-MM-DD.`
- Do not silently edit ADRs in the same pass unless the task scope includes it.

---

## Initialization path adjustments

When README edit changes init path (see scaffolding map):

| Transition | Backlog effect |
| --- | --- |
| Small → medium | Add `PROJ-ADR-*`, `PROJ-DOC-*`, `PROJ-STACK-*` rows; mark new gates **required for handoff** per [`handoff.md`](./handoff.md) |
| Medium → large | Add `PROJ-AGENT-*`, `PROJ-ORCH-*`; optional rows become active |
| Large → small | **Ask user** before cancelling ADR/agent/orch tasks — may still want artifacts |

---

## `docs/roadmap/README.md` sync

After material README edits, update the roadmap index:

| Field | Update when |
| --- | --- |
| Handoff status | Any **required for handoff** task opened or closed |
| Last reviewed | Material README sync completed |
| Open gates list | Optional one-line summary of blocking `PROJ-HANDOFF-*` |

Example index line:

```markdown
**Handoff status:** Blocked — open `PROJ-HANDOFF-001`, `PROJ-ADR-001` (required for handoff).
```

---

## Coordination with README workflow

| Step | Document |
| --- | --- |
| Audit README before edit | [`../target-readme/audit.md`](../target-readme/audit.md) |
| Apply outline / draft | [`../target-readme/outline.md`](../target-readme/outline.md) |
| Map new project → initial rows | [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) |
| README self-containment | [`../target-readme/handoff-checklist.md`](../target-readme/handoff-checklist.md) |
| Jarvis initialization complete | [`handoff.md`](./handoff.md) |

**Order:** README edit → **this doc** (backlog) → fix artifacts → README handoff checklist → project handoff criteria.

---

## Human input (pause)

Stop and ask before:

- Cancelling **required for handoff** tasks to match a README downgrade.
- Reopening tasks that imply reversing an **Accepted** ADR.
- Removing `docs/roadmap/` from the README while open `PROJ-*` tasks remain.
- Marking initialization complete when material README edits left backlog stale.

---

## Anti-patterns

| Anti-pattern | Fix |
| --- | --- |
| README says "we use Redis" with no `PROJ-STACK-*` / ADR | Add tasks or remove claim |
| Duplicate `PROJ-DOC-*` for same link | Merge titles; one ID |
| New ID for same work after cancel | Reopen from **Cancelled** if scope returned |
| Only updating chat | Update `backlog.md` in same session |
| Product feature in setup backlog | Move to product roadmap doc; link from README § Roadmap |
