# Stack confirmation and recording

Turns the detection draft from [`detection.md`](./detection.md) into **durable target-project facts**: README § Technology Stack, `docs/stack/stack-profile.md`, and `PROJ-STACK-*` backlog rows.

## When to run

- After detection pass completes for the scoped package root.
- After target README draft exists **or** intake Q5 answered (greenfield).
- Again when README § Stack changes materially ([`readme-sync.md`](../target-roadmap/readme-sync.md) → update stack-profile and reopen `PROJ-STACK-*` if needed).

## Confirmation batch (default)

Present **one message** with:

1. **Summary table** — capability fields + confidence + evidence paths (abbreviated).
2. **Assumptions** — all `medium` fields explicitly labeled.
3. **Questions** — only items in [Ask table](#ask-table) below.
4. **Corrections prompt** — "Reply with corrections or confirm."

Skip questions already answered in intake or unambiguous README text **unless** manifests contradict README.

### Ask table

| # | Question | When |
| --- | --- | --- |
| S1 | **Confirm or correct** the detected stack summary (language, framework, package manager, primary datastore). | Always once per init (batch); skip only if user just answered intake Q5 with same facts and detection agrees |
| S2 | **Which package root is the product?** (path) | Monorepo or multiple apps ([`detection.md` § Monorepos](./detection.md#monorepos)) |
| S3 | **Is the primary framework** X **or** Y? | `conflict` on framework field |
| S4 | **Primary durable datastore** (if not evident from files)? | No datastore signals; README mentions "database" vaguely |
| S5 | **Any stack components intentionally excluded** from agent guidance? (e.g. legacy Angular app maintained but not greenfield) | User mentioned augmenting existing code; multiple frameworks detected |

### Do not ask

| Topic | Reason |
| --- | --- |
| Package manager when lockfile exists | [`intake-questions.md`](../target-readme/intake-questions.md) |
| Framework tutorial / MCP preferences | Optional in [`selection.md`](./selection.md) pause table — default: upstream refs only |
| Exact library versions | README outline — versions only when architectural |
| Every test layer setup | `JR-STACK-004` |
| Deployment secrets / env values | Security; `JR-STACK-005` |

## Conflicts

A **conflict** exists when:

- Two **high** framework signals apply to the same package root.
- README § Technology Stack names a framework that **manifests contradict**.
- User intake answer disagrees with **high** detection.

**Rule:** Do not write README or `stack-profile.md` until S1/S3 resolves the conflict. Record the open question on `PROJ-STACK-000` in target `docs/roadmap/backlog.md` if the session pauses.

## Recording (target repository)

After user confirms or provides corrections:

### 1. Create or update `docs/stack/stack-profile.md`

- Copy from [`stack-profile.example.md`](../templates/stack-scaffolding/stack-profile.example.md).
- Fill **Confirmed** table; remove unused capability rows.
- Set `last_verified` to ISO date and list evidence paths (not Jarvis paths).
- **No links to Jarvis** — handoff rule.

### 2. Update README § Technology Stack

- Align bullets with stack-profile (high level only).
- If README predates stack-profile, prefer **user-confirmed** facts over stale README prose.

### 3. Update target `docs/roadmap/backlog.md`

Minimum rows:

```markdown
- [x] `PROJ-STACK-000`: Record stack profile from repository detection and user confirmation. **required for handoff**
  - Evidence: `docs/stack/stack-profile.md` verified YYYY-MM-DD; paths: …
```

Spawn follow-ups per [`scaffolding-map.md`](../target-readme/scaffolding-map.md):

- `PROJ-STACK-001` — package manager and validation commands ([`commands.md`](./commands.md), `JR-STACK-003`)
- `PROJ-STACK-002` — framework-specific rules/docs ([`selection.md`](./selection.md))
- `docs/stack/source-documentation.md` — when production code exists ([`universal-docs`](../universal-docs/README.md))

### 4. Link from README § Documentation

Add `docs/stack/stack-profile.md` when the file is created (medium/large init default; small init when any `PROJ-STACK-*` exists).

## Greenfield (no manifests)

| Step | Action |
| --- | --- |
| Detection | Skip file pass; mark all fields `low` |
| Questions | Use intake Q5 ([`intake-questions.md`](../target-readme/intake-questions.md)) |
| Recording | Write README § Stack from answers; create stack-profile with `evidence: user intake YYYY-MM-DD` |
| Backlog | `PROJ-STACK-000` open until manifests exist and profile is re-verified |

Do not invent framework or language to unblock scaffolding.

## Re-verification triggers

Re-run detection + confirmation when:

- User adds or removes a major framework dependency.
- README § Stack edited materially.
- Lockfile or manifest changes ecosystem (npm → pnpm, CRA → Vite).
- Monorepo layout changes which package is primary.

Update `last_verified` and evidence paths; add a sub-bullet under the relevant `PROJ-STACK-*` task.

## Agent efficiency notes

- **One batch** per init reduces round-trips; do not ask S1 field-by-field.
- **stack-profile.md** is the detailed SoT; README stays scannable — agents read README first, then stack-profile for stack work.
- Store **evidence paths** in stack-profile so cold sessions avoid re-scanning the whole repo unless re-verification triggers apply.
