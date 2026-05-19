# Minimum read sets per role (`JR-AGENT-003`)

Canonical guidance for **target-project** agent sessions: what to read **first** so fresh-context runs stay correct without loading the whole repository. This document generalizes patterns from mature orchestration setups; it does **not** embed stack commands or product domains.

**Platform task:** `JR-AGENT-003`  
**Related:** [`README.md`](./README.md) (roster + contracts), [`handoff-prompts.md`](./handoff-prompts.md) (paste blocks — reference read sets, do not duplicate tables), [`../orchestration/artifact-ownership.md`](../orchestration/artifact-ownership.md) (mandatory cross-artifact reads), [`../orchestration/loops-and-rework.md`](../orchestration/loops-and-rework.md) (remediation variants), [`../orchestration/init-paths.md`](../orchestration/init-paths.md) (run tier skips)

**Target copy source:** [`../templates/universal-agents/minimum-read-sets.example.md`](../templates/universal-agents/minimum-read-sets.example.md)

## Mandatory inputs vs minimum read set

| Concept | Where it lives | Purpose |
| --- | --- | --- |
| **Mandatory inputs** | Per-role contract **Inputs** section | Completeness — everything the role may need before handoff is valid |
| **Minimum read set** | Target `.cursor/agents/INDEX.md` § Minimum read sets (default) or orchestration guide appendix | **Efficiency** — ordered read list for a typical fresh session; defers deep reads until needed |

Agents MUST satisfy mandatory inputs before claiming an output contract is met. The minimum read set is the **default order**; read additional mandatory inputs when the task touches those boundaries (ADR citations, checklist domains, matrix layers).

Do **not** duplicate full mandatory-input lists inside read-set tables — link to the role contract instead.

## Where targets publish read sets

| Placement | When to use |
| --- | --- |
| **`.cursor/agents/INDEX.md` § Minimum read sets** (default) | Large init, any project with agent contracts — one discoverability point next to roster and gates |
| **Orchestration guide appendix** | Team prefers a long human narrative guide; INDEX links to the appendix |
| **Both** | Allowed only if INDEX holds the tables and the guide holds tier/path nuance — avoid contradictory duplicates |

Jarvis templates put tables in [`minimum-read-sets.example.md`](../templates/universal-agents/minimum-read-sets.example.md) for paste into INDEX. Role contract templates intentionally **omit** read-set tables (`JR-AGENT-002`); they end with a pointer to INDEX or the orchestration guide.

**Self-containment:** Read sets list **target-root-relative** paths only. Never list Jarvis `docs/` paths, `JR-*` IDs, or sibling-repo URLs as mandatory reads ([`../orchestration/self-containment.md`](../orchestration/self-containment.md), handoff **IND-08**).

## Design principles

1. **Task folder first** — For orchestrated runs, start with `task-manifest.json` and the artifacts that role owns or consumes; project-wide docs are second unless Gate 0 has no manifest yet.
2. **Read for decisions, not inventory** — Open source files and ADRs when the plan, ACs, or remediations name them; do not preload unrelated modules.
3. **Tier-aware** — `risk_tier.level` and `skipped_stages` shrink or swap rows (see [Run tier modifiers](#run-tier-modifiers)); record path letter in manifest `flags` for small runs.
4. **Session variants** — Remediation Builder, Gate 6 Orchestrator resume, and human rework use different rows than first entry (see [Session variants](#session-variants)).
5. **Checklist slicing** — Validator (and Planner when scoping ACs) reads **applicable sections only** of the validation checklist — not the full file on every run.
6. **Commands from evidence** — Use `docs/stack/commands.md` (or README § Development) for command strings; never invent scripts in read-set prose.

## Project baseline (any agent)

Applies to orchestrated and non-orchestrated work unless the manifest records an approved exception (human pause — see below).

| Always read | Read when applicable |
| --- | --- |
| Root `README.md` (product intent, stack summary, doc map) | `docs/roadmap/README.md` when resuming multi-session platform work |
| | `docs/roadmap/backlog.md` — open `PROJ-*` for the current objective and dependencies |
| | `adrs/INDEX.md` when the change may touch durable architecture |
| | `.cursor/rules/index.md` when editing code or rules |

**Non-orchestrated changes:** Follow the same baseline plus topic rules and Accepted ADRs for the touched area. Merge-ready claims still use checklist **MG-*** rows per target workflow rules — no task folder required.

## Orchestrator

**Contract:** `.cursor/agents/orchestrator.md` — mandatory inputs include manifest, stage outputs, and post-Validator `validation-report.md`.

### Standard session (routing between stages)

| Always read | Read when applicable |
| --- | --- |
| `.cursor/orchestrations/{task-id}/task-manifest.json` | Prior stage artifact named in `completed_stages[].output_artifacts` |
| Own role contract (orchestrator.md) | `validation-report.md` after Validator |
| Target orchestration guide — handoff / skip / loop policy only | `human-approval.md` when resuming Gate 6 or rework |
| | `plan.md` when checking Gate 1–2 or Planner open questions |
| | `build-log.md`, `test-report.md` before Gate 6 evidence summary |

### Gate 6 / `awaiting_human` resume

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json`, `human-approval.md` (if present) | `validation-report.md` when Validator ran |
| `build-log.md` | `test-report.md` when Tester ran |
| Orchestration guide — Gate 6 and post-approval commit policy | `plan.md` only if human rework may require Planner |

Do **not** reload full ADR corpora for Gate 6 unless the human question is architectural.

## Planner

**Contract:** `.cursor/agents/planner.md`.

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json` (`objective`, `locked_artifacts`, `risk_tier`, `flags`) | Each **Accepted** ADR cited or plausibly relevant — **full file** before citing in `plan.md` |
| `adrs/INDEX.md`, `adrs/GOVERNANCE.md` | Source paths implied by objective (enough for real file map) |
| `.cursor/orchestrations/_template/plan.md`, `acceptance-criteria.md` (shape) | `REPLACE_WITH_VALIDATION_CHECKLIST_PATH` — sections for domains the objective touches |
| Verified commands doc (e.g. `docs/stack/commands.md`) — command names only until planning validation commands | `test-matrix.md` template + testing-strategy doc when matrix required (`medium`/`large` or testable code) |
| | Open `PROJ-*` dependencies in `docs/roadmap/backlog.md` that block the objective |

**Skip Planner on run:** Orchestrator does not invoke Planner; read sets for Planner do not apply. Builder must not rely on new plan sections unless human replans.

## Builder

**Contract:** `.cursor/agents/builder.md`.

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json` (`locked_artifacts`) | Accepted ADRs **cited in `plan.md`** — full read when implementing that boundary |
| `plan.md`, `acceptance-criteria.md` | Files in plan **Component/file map** (open on demand per file, not whole tree) |
| `.cursor/orchestrations/_template/build-log.md` (shape) | Topic `.mdc` rules for paths being edited (from `.cursor/rules/index.md`) |
| | Prior `build-log.md` + `validation-report.md` **Required remediations** on remediation loops only |

Builder does **not** read the full validation checklist or unrelated ADRs unless the plan requires it.

## Tester

**Contract:** `.cursor/agents/tester.md`.

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json` (`task_id`, `risk_tier`) | `test-matrix.md` — **Planned coverage** when matrix required |
| `acceptance-criteria.md` | Testing-strategy doc / matrix template for layer IDs |
| `build-log.md` (**Known gaps**, files changed) | `plan.md` — scope/context only; do not add requirements |
| Existing tests adjacent to changed paths | Full `plan.md` when AC-to-layer mapping is unclear |

## Validator

**Contract:** `.cursor/agents/validator.md`.

| Always read | Read when applicable |
| --- | --- |
| `task-manifest.json` (`loop_count`, `max_loops` — context only) | `test-matrix.md` when Planner/Tester used a matrix |
| `plan.md`, `acceptance-criteria.md`, `build-log.md`, `test-report.md` | Each Accepted ADR cited in `plan.md` |
| Validation checklist — **applicable sections only** | Source and test files named in plan, build log, and test report |
| `.cursor/agents/validator.md` + target workflow-gates rule (MG-* semantics) | Alignment-gaps doc when plan or ADRs reference known drift |

Validator does **not** re-read the entire codebase; audit evidence targets named files and AC coverage claims.

## Session variants

| Variant | Roles affected | Adjustments |
| --- | --- | --- |
| **Remediation loop** (Validator **FAIL**) | Builder → Tester → Validator | Builder: add prior `build-log.md` + `validation-report.md` **Required remediations**; plan/ACs frozen. Tester/Validator: replace prior reports — read latest Builder/Tester outputs only. See [`../orchestration/loops-and-rework.md`](../orchestration/loops-and-rework.md). |
| **Human rework** (`rejected_rework`) | Orchestrator → Builder or Planner | Orchestrator reads `human-approval.md` + manifest `rework_history`. Scope expansion → Planner; fix-only → Builder remediation set. |
| **Planner skipped** | Builder | Builder treats manifest `objective` + any Orchestrator directive as scope; still read cited ADRs if objective names architecture. |
| **Tester skipped** (small Path B) | Validator | Validator reads `build-log.md`; **MG-04** evidence may live in build log + PR — document in validation report. |
| **Validator skipped** (small Path A) | Orchestrator, human | No `validation-report.md`; Orchestrator Gate 6 uses test report + checklist substitute per init-paths / orchestration guide. |

## Run tier modifiers

Apply after the role baseline. Full skip matrices: [`../orchestration/init-paths.md`](../orchestration/init-paths.md) Part B.

| `risk_tier.level` | Typical pipeline | Read set impact |
| --- | --- | --- |
| **small** | Path A/B/C in orchestrator contract | Omit Planner/Tester/Validator rows when stage skipped; manifest `skipped_stages` is authoritative |
| **medium** | Planner → Builder → Tester → Validator | Full stage read sets; `test-matrix.md` usually required for testable code |
| **large** | Same as medium; may loop phases | Planner may split phases — Builder reads **current phase** section of `plan.md` first |

## Copy and adapt (Jarvis → target)

1. Complete role contracts and INDEX ([`README.md`](./README.md) § Copy and adapt).
2. Copy [`minimum-read-sets.example.md`](../templates/universal-agents/minimum-read-sets.example.md) tables into `.cursor/agents/INDEX.md` § **Minimum read sets** (or orchestration guide appendix).
3. Replace `REPLACE_WITH_*` paths with target doc paths (checklist, commands, testing-strategy, alignment gaps).
4. Add one row per stack-specific “always read” only when the team agrees (e.g. framework MCP doc) — prefer pointers in `.cursor/rules/` over bloating INDEX.
5. Verify handoff **IND-08**: no Jarvis paths in read sets.
6. Record `PROJ-AGENT-*` evidence in target backlog when scaffolding agents.

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- **Omitting project baseline reads** (README / roadmap) for orchestrated runs via a standing manifest flag — risks wrong scope on fresh sessions.
- **Splitting contradictory read sets** across INDEX and orchestration guide (two sources of truth).
- **Expanding “Always read”** to entire `adrs/` trees or full checklist files for every Builder/Tester session.
- **Pointing read sets at Jarvis** platform docs or template URLs instead of copied target files.

Routine paste of example tables, placeholder replacement, and aligning with contracts do **not** require extra approval.

## Decisions recorded for `JR-AGENT-003`

| Topic | Default | Notes |
| --- | --- | --- |
| Canonical platform doc | **`docs/universal-agents/minimum-read-sets.md`** | This file |
| Target publication | **`.cursor/agents/INDEX.md` § Minimum read sets** | Orchestration guide appendix optional for nuance |
| Contract templates | **No read-set tables** | Pointer only; mandatory inputs stay in **Inputs** |
| Table shape | **Always read** \| **Read when applicable** | Matches efficient WFD-style contracts |
| Project baseline | README → roadmap index/backlog → ADRs index when architectural | Aligns with [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) |
| ADR depth | Planner + Validator: cited Accepted ADRs in full; Builder: cited only | Avoids ADR corpus preload |
| Checklist depth | Applicable sections only | Validator and Planner when scoping |
| Remediation | Builder adds validation **Required remediations**; reports replaced per loop | [`loops-and-rework.md`](../orchestration/loops-and-rework.md) |
| Jarvis paths in targets | **Forbidden** as mandatory reads | **IND-08** |

## Related material

| Resource | Role |
| --- | --- |
| [`../templates/universal-agents/minimum-read-sets.example.md`](../templates/universal-agents/minimum-read-sets.example.md) | Paste-ready target tables |
| [`../templates/universal-agents/INDEX.example.md`](../templates/universal-agents/INDEX.example.md) | Roster + § Minimum read sets hook |
| [`../orchestration/artifact-ownership.md`](../orchestration/artifact-ownership.md) | Consumer “must read before acting” matrix |
| [`../roadmap/backlog.md`](../roadmap/backlog.md) | `JR-AGENT-003` task |
