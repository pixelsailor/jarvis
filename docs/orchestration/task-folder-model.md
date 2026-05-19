# Task-folder model (`JR-ORCH-001`)

Generalized orchestration layout for **target projects**. One **task folder** per multi-agent run holds all pipeline state and stage outputs so work can resume in a fresh session without chat memory.

**Companion:** [`task-manifest.md`](./task-manifest.md) (`JR-ORCH-002`) — `task-manifest.json` field contract.  
**Templates:** [`../templates/orchestration/`](../templates/orchestration/).

## Why task folders exist

- **Deterministic resumes:** The next agent reads the folder, not the previous chat.
- **Explicit handoffs:** Each role writes named artifacts; the Orchestrator routes via `task-manifest.json`.
- **Separation from application source:** Orchestration files record process and evidence — they are not product code.
- **Right-sized runs:** Small changes can skip stages when the Orchestrator records tier and skips in the manifest (see [Risk tiers](#risk-tiers)); large init or architecture work uses the full path.

## Canonical path

All run state for a given task lives under:

```text
.cursor/orchestrations/{task-id}/
```

| Rule | Detail |
| --- | --- |
| **Folder slug** | Must **exactly equal** `task_id` in `task-manifest.json` (e.g. folder `acme-042` → `"task_id": "acme-042"`) |
| **`task_id` pattern** | `{project-slug}-NNN` — `project-slug` is set at project init (lowercase, hyphenated, no spaces); `NNN` is zero-padded or sequential per team convention |
| **No alternate roots by default** | Do not scatter orchestration state to repo-root scratch files or chat-only plans |
| **Template source** | Copy from `.cursor/orchestrations/_template/` (seeded from Jarvis [`../templates/orchestration/_template/`](../templates/orchestration/_template/)) |

### Choosing `project-slug`

Set once when orchestration is first scaffolded (typically **large** init). Examples: repository name (`acme-web`), product codename (`ledger`), or user-provided short slug. Document the slug in the target orchestration guide and optionally in an ADR or `docs/roadmap/README.md` so agents do not invent conflicting prefixes.

## Lifecycle (human-readable)

```text
Orchestrator → Planner → Builder → Tester → Validator → Orchestrator
```

| Role | In `pipeline` array? | Typical manifest `current_agent` |
| --- | --- | --- |
| Orchestrator | **No** (bookend) | `orchestrator` at intake, after validation, and Gate 6 |
| Planner | Yes | `planner` |
| Builder | Yes | `builder` |
| Tester | Yes | `tester` |
| Validator | Yes | `validator` |

Default `pipeline`:

```json
["planner", "builder", "tester", "validator"]
```

After Validator **PASS** or **PASS_WITH_NOTES**, the Orchestrator sets `status` to `awaiting_human` and `current_agent` to `orchestrator` for human approval — not the next `pipeline` entry.

Stage **contracts** (minimum read sets, write permissions) belong in `.cursor/agents/` (`JR-AGENT-*`). This document defines **where** artifacts live, not per-role prose.

## Artifact set

Files under `.cursor/orchestrations/{task-id}/`:

| File | Primary owner | Purpose |
| --- | --- | --- |
| `task-manifest.json` | Orchestrator | Authoritative run state ([`task-manifest.md`](./task-manifest.md)) |
| `plan.md` | Planner | Scope, file map, ADR implications, validation commands, risks |
| `acceptance-criteria.md` | Planner | Verifiable **AC-01**, **AC-02**, … list |
| `test-matrix.md` | Planner → Tester | Planned coverage (Planner); actual coverage (Tester) when required |
| `build-log.md` | Builder | Files changed, command evidence, deviations |
| `test-report.md` | Tester | AC-to-test map, commands, gaps |
| `validation-report.md` | Validator | Verdict, AC/ADR/checklist audit, remediations |
| `human-approval.md` | Orchestrator | Gate 6 record; mirrors manifest `human_approval` |

**Cross-cutting docs (target repo, not inside task folder):**

- `docs/validation-checklist.md` — merge-ready **MG-*** and domain rows
- `docs/pr-and-commit-guide.md` — PR/commit evidence format
- Stack commands — `docs/stack/commands.md` or README § Development (**verified scripts only**)

Markdown artifact **templates** ship in `_template/` when the target scaffolds orchestration (`PROJ-ORCH-*`). Full section contracts, freeze rules, and loop behavior: [`artifact-ownership.md`](./artifact-ownership.md) (`JR-ORCH-003`). This table is the stable filename contract agents rely on.

### Bootstrap from `_template`

1. Copy `.cursor/orchestrations/_template/*` → `.cursor/orchestrations/{task-id}/`.
2. Set `task-manifest.json`: `task_id`, `objective`, `locked_artifacts`, initial `risk_tier` and `gate_status`.
3. Orchestrator invokes the first stage per tier (often `planner`, or `builder` on justified small paths).
4. Each role completes its owned files before the Orchestrator advances `current_agent`.

See [`../templates/orchestration/_template/README.example.md`](../templates/orchestration/_template/README.example.md).

## Gates, merge-ready, and handoff (do not conflate)

Three vocabularies — full contract in [`gates-and-checks.md`](./gates-and-checks.md) (`JR-ORCH-004`):

| Vocabulary | Tracks | Recorded in |
| --- | --- | --- |
| **Lifecycle gates (0–6)** | Pipeline stage completion | `task-manifest.json` → `gate_status` |
| **Merge-ready checks (MG-01–MG-05)** | Whether work may be claimed **merge-ready** | `validation-report.md`, PR body, checklist |
| **Handoff checks (PROJ-HANDOFF-*)** | Target independence from Jarvis | `docs/handoff-self-containment.md`, target backlog |

**Lifecycle Gate 5** (validation green) ≠ **MG-05** (tooling). **Lifecycle Gate 6** (human approval) follows a green merge-ready path; it does not replace MG-* items. Handoff completion does not satisfy merge-ready for a feature run.

Checklist mapping: [`../templates/universal-validation/validation-checklist.md`](../templates/universal-validation/validation-checklist.md) — orchestration appendix. Target narrative: orchestration guide + `.cursor/rules/workflow-gates.mdc`.

## Risk tiers

Orchestrator sets `risk_tier.level` before skipping stages:

| Tier | Typical use | Minimum path (examples) |
| --- | --- | --- |
| `small` | One or two files, low architectural risk | Orchestrator → Builder → Tester and/or Validator → Orchestrator → Gate 6 |
| `medium` | Single feature area, several files | Full `pipeline` once |
| `large` | Cross-cutting, ADR-governed, migrations, security boundaries | Planner + phased Builder → Tester → Validator loops |

Skipped stages **must** appear in `risk_tier.skipped_stages`, matching `gate_status` keys set to `skipped`, and rationale in `risk_tier.rationale` or `flags`. **Gate 6 is not skipped** for code-changing work.

Small-run path details (paths A/B/C; Validator vs Tester skipped) are in [`init-paths.md`](./init-paths.md) (`JR-ORCH-005`) and the target orchestration guide; do not skip Validator on architecture-touching work without user approval.

## Edit ownership (summary)

See [`artifact-ownership.md`](./artifact-ownership.md) for section contracts and freeze after handoff; [`loops-and-rework.md`](./loops-and-rework.md) for remediation/rework routing and counters.

| Actor | May write |
| --- | --- |
| Orchestrator | `task-manifest.json`, `human-approval.md` |
| Planner | `plan.md`, `acceptance-criteria.md`, planned `test-matrix.md` |
| Builder | `build-log.md` only (implementation in app source per plan) |
| Tester | `test-report.md`, actual `test-matrix.md` |
| Validator | `validation-report.md` only |
| All roles | Read manifest; respect `locked_artifacts` |

Only the Orchestrator updates routing fields (`current_agent`, `gate_status`, `completed_stages`, loops, approval). Other agents report facts in owned artifacts; the Orchestrator mirrors summaries when routing.

## Git policy

| Phase | Default |
| --- | --- |
| **During run** | Task folder **local**; agents do **not** commit unless the user explicitly asks |
| **PR evidence** | Paste summaries from `test-report.md` / `validation-report.md` and checklist rows |
| **After Gate 6** | Orchestrator **asks** whether to commit application changes and optionally the task folder |
| **Team override** | If the project tracks orchestration in git, document that in the target orchestration guide — Jarvis still asks before first commit of a filled folder |

Align with [`../templates/universal-pr-commit/pr-and-commit-guide.md`](../templates/universal-pr-commit/pr-and-commit-guide.md).

## Fresh-session handoffs

Each Orchestrator handoff should include a **Next Agent Directive** (role, task folder path, risk tier, objective, scope allowlist, required reads, validation commands from **verified** manifests, outputs, stop conditions). Canonical templates: [`../universal-agents/handoff-prompts.md`](../universal-agents/handoff-prompts.md) (`JR-AGENT-004`); target paste [`../templates/universal-agents/handoff-prompts.example.md`](../templates/universal-agents/handoff-prompts.example.md) into the orchestration guide — not duplicated in role contracts.

Treat **NEW SESSION: YES** as the default for Planner, Builder, Tester, and Validator.

## What not to put in task folders

- Application source (belongs in normal tree per `plan.md`)
- Jarvis repository links or `JR-*` IDs
- WFD-specific gap IDs, product language, or package scripts not verified in the target repo
- Duplicate backlog state — resumable **setup** work stays in `docs/roadmap/backlog.md` (`PROJ-*`); task folders are for **orchestrated execution** of a defined objective

## Precedence when documents disagree

1. **Accepted ADRs** and target `adrs/GOVERNANCE.md`
2. Target workflow / merge-ready rules (when present)
3. **Role contracts** — `.cursor/agents/*.md`
4. **Templates** — `.cursor/orchestrations/_template/`
5. Target orchestration guide (human quickstart)

## Related

| Doc | Role |
| --- | --- |
| [`task-manifest.md`](./task-manifest.md) | JSON schema |
| [`artifact-ownership.md`](./artifact-ownership.md) | Per-file contracts (`JR-ORCH-003`) |
| [`init-paths.md`](./init-paths.md) | Init and run-sizing paths (`JR-ORCH-005`) |
| [`self-containment.md`](./self-containment.md) | Copy sanitize and **ORCH-IND-*** (`JR-ORCH-007`) |
| [`README.md`](./README.md) | Index and decisions |
| [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) | When to scaffold orchestration (large init) |
