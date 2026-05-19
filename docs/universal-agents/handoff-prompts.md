# Handoff prompts and Next Agent Directives (`JR-AGENT-004`)

Canonical guidance for **target-project** Orchestrator handoffs: copy-paste **Next Agent Directive** blocks and related prompts that let a **fresh session** run the next role without chat memory. Generalizes WFD-style directive discipline; no product domains, gap IDs, or package scripts.

**Platform task:** `JR-AGENT-004`  
**Related:** [`README.md`](./README.md) (roster + contracts), [`minimum-read-sets.md`](./minimum-read-sets.md) (what to read — this doc defines **what to paste**), [`../orchestration/task-folder-model.md`](../orchestration/task-folder-model.md) (fresh-session policy), [`../orchestration/loops-and-rework.md`](../orchestration/loops-and-rework.md) (loop addenda), [`../orchestration/artifact-ownership.md`](../orchestration/artifact-ownership.md) (output contracts)

**Target copy source:** [`../templates/universal-agents/handoff-prompts.example.md`](../templates/universal-agents/handoff-prompts.example.md)

## Where targets publish prompts

| Placement | Use |
| --- | --- |
| **Target orchestration guide** (default: `docs/ORCHESTRATED_DEVELOPMENT.md`) § **Next Agent Directive** | Canonical copy-paste templates — **single source of truth** |
| **Optional** § **Prompt quickstart** in the same guide | One-line activation table (goal → role → first artifact) |
| **`.cursor/agents/orchestrator.md`** | **Pointer only** — rule that every downstream handoff includes a complete directive; **do not** duplicate the full template (WFD drift: duplicate blocks in contracts + guide confuse agents) |
| **Other role contracts** | Activation + output contract only; no directive templates |
| **`.cursor/agents/INDEX.md`** | Link to orchestration guide § directives; optional one-line “paste from guide” |

Jarvis paths, `JR-*` IDs, and Jarvis repository URLs MUST NOT appear in target handoff text (**IND-08**).

## Design principles

1. **Paste-ready blocks** — Orchestrator fills placeholders; the next agent copies the whole block into a new session (or delegated run).
2. **`NEW SESSION: YES`** — Default for Planner, Builder, Tester, and Validator. Orchestrator resume (Gate 6, routing) may reuse context when the human directs; stage agents assume a cold start.
3. **Manifest + artifacts over chat** — Directives name `task_id`, folder path, tier, controlling artifacts, and scope allowlist; they do not restate full plan prose.
4. **Commands from evidence** — **VALIDATION COMMANDS** lists exact strings from `docs/stack/commands.md` or README § Development only.
5. **Read sets by reference** — Point to `.cursor/agents/INDEX.md` § Minimum read sets (or orchestration guide appendix); do not duplicate full read tables inside every directive.
6. **Loops are addenda** — Remediation and human rework append a **Loop context** block; base template stays stable.

## Base Next Agent Directive (all stage handoffs)

Orchestrator issues this shape when routing to Planner, Builder, Tester, or Validator. Replace angle-bracket placeholders; delete unused bullets.

```text
NEXT ROLE:
<Planner | Builder | Tester | Validator>

NEW SESSION:
YES

TASK FOLDER:
.cursor/orchestrations/<task-id>/

MANIFEST:
task-manifest.json — read first; do not edit unless you are the Orchestrator

RISK / TIER:
<small | medium | large> — <one-line rationale>

SMALL-RUN PATH (if risk_tier.level is small):
<Path A | B | C | none> — <skipped stages from manifest risk_tier.skipped_stages>

OBJECTIVE:
- <success outcome from manifest objective, tightened if needed>

CONTRACT CHECK:
- Controlling artifacts: <e.g. plan.md, acceptance-criteria.md>
- Scope allowlist: <files/folders from plan map or remediation list>
- Exit criteria: <role output contract + behavioral checks>
- Locked artifacts: <from manifest locked_artifacts or "none">

REQUIRED INPUTS (read before acting):
- .cursor/orchestrations/<task-id>/task-manifest.json
- <role-specific artifacts — see role variants below>
- Minimum read set: <path to INDEX § Minimum read sets or orchestration guide appendix>

VALIDATION COMMANDS (verified only):
- <exact commands from docs/stack/commands.md or README § Development>
- If none apply: state "none" and why

OUTPUTS (this session):
- <role-owned artifact path(s)>
- <summary / command_evidence expectations per artifact-ownership doc>

MANIFEST RULE:
Do not edit task-manifest.json — Orchestrator owns routing fields.

STOP CONDITIONS:
- Objective or ACs are contradictory
- Work requires files outside scope allowlist
- Accepted ADR conflict with plan or implementation
- Required command is missing, unverified, or impossible to run
- Dependency or toolchain change needed without explicit human approval
- Output contract cannot be met — halt and report to Orchestrator
```

## Role variants (fill CONTRACT CHECK and REQUIRED INPUTS)

Use the base block plus these defaults. Customize scope allowlist and commands per run.

### Planner

| Field | Typical content |
| --- | --- |
| Controlling artifacts | `task-manifest.json` only (Planner creates `plan.md`, `acceptance-criteria.md`, planned `test-matrix.md`) |
| REQUIRED INPUTS | `adrs/INDEX.md`, `adrs/GOVERNANCE.md`; cited Accepted ADRs in full when relevant; validation checklist — applicable sections; commands doc; testing-strategy when matrix required |
| OUTPUTS | `plan.md`, `acceptance-criteria.md`; `test-matrix.md` when medium/large or testable code |
| STOP CONDITIONS | + Open questions that block Builder without human resolution |

### Builder

| Field | Typical content |
| --- | --- |
| Controlling artifacts | `plan.md`, `acceptance-criteria.md`; `locked_artifacts` respected |
| REQUIRED INPUTS | Cited ADRs from plan (full read per ADR); topic rules for touched paths; prior `build-log.md` + `validation-report.md` **Required remediations** on remediation loops only |
| OUTPUTS | `build-log.md`; implementation in repo per plan file map |
| STOP CONDITIONS | + Plan requires locked or out-of-scope files; replan needed (route Orchestrator, do not expand scope) |

### Tester

| Field | Typical content |
| --- | --- |
| Controlling artifacts | `acceptance-criteria.md`, `build-log.md`; `test-matrix.md` when matrix required |
| REQUIRED INPUTS | `plan.md` for scope context only; testing-strategy / matrix template; tests adjacent to changed paths |
| OUTPUTS | `test-report.md`; actual coverage in `test-matrix.md` when applicable |
| STOP CONDITIONS | + Cannot map AC to a test layer without Orchestrator/human decision |

### Validator

| Field | Typical content |
| --- | --- |
| Controlling artifacts | `plan.md`, `acceptance-criteria.md`, `build-log.md`, `test-report.md` |
| REQUIRED INPUTS | Validation checklist — **applicable sections only**; cited Accepted ADRs; workflow-gates rule for **MG-*** semantics |
| OUTPUTS | `validation-report.md` with verdict **PASS**, **PASS_WITH_NOTES**, or **FAIL** |
| STOP CONDITIONS | + **FAIL** must list **Required remediations**; do not claim merge-ready |

## Loop and rework addendum

Append after the base block when `loop_count > 0` or `rework_count > 0` (see [`../orchestration/loops-and-rework.md`](../orchestration/loops-and-rework.md)).

```text
LOOP CONTEXT:
- Type: <validation remediation | human rework>
- loop_count: <n> / max_loops: <m>
- rework_count: <n> (if human rework)
- Prior verdict or approval: <e.g. validation FAIL — see validation-report.md § Required remediations>

BUILDER (remediation or fix-only rework):
- Read: validation-report.md § Required remediations OR human-approval.md § Rework directive
- Do not expand scope beyond allowlist + remediations
- Append build-log.md (do not replace prior sections)

TESTER / VALIDATOR (after remediation Builder):
- Replace prior test-report.md / validation-report.md for this invocation
- Full pipeline: Builder → Tester → Validator unless documented approved exception
```

Orchestrator MUST NOT increment `loop_count` except on Validator **FAIL** → Builder per loop policy.

## Gate 6 human evidence (not a stage directive)

When `status` is `awaiting_human`, the Orchestrator presents evidence to the human (chat or ticket). This is **not** pasted as a Next Agent Directive to another agent.

```text
GATE 6 — HUMAN APPROVAL REQUEST

TASK:
<task-id> — <objective one line>

RISK / SKIPS:
<tier> — <skipped_stages / path letter if small>

EVIDENCE:
- Files changed: <from build-log.md>
- AC coverage: <from test-report.md when Tester ran>
- Validation: <verdict + summary from validation-report.md when Validator ran; or substitute per small-run path>
- Commands run / not run: <from build-log, test-report, command_evidence>
- Rework history: <if rework_count > 0>

MERGE-READY (MG-*):
<which MG rows satisfied; note limited merge-ready if small Path A>

REQUEST:
Approve | Approve with conditions | Reject | Reject — rework (scope: Builder | Planner)
```

Record outcome in `human-approval.md` and manifest `human_approval`; mirror fields per [`../orchestration/artifact-ownership.md`](../orchestration/artifact-ownership.md).

## Prompt quickstart (optional table)

Targets may add a short table in the orchestration guide for humans learning the pipeline:

| Goal | Start role | Contract | First artifacts |
| --- | --- | --- | --- |
| New run | Orchestrator | `orchestrator.md` | Create folder + manifest + first directive |
| Plan | Planner | `planner.md` | `plan.md`, `acceptance-criteria.md` (+ matrix if required) |
| Implement | Builder | `builder.md` | `build-log.md` |
| Test | Tester | `tester.md` | `test-report.md` |
| Audit | Validator | `validator.md` | `validation-report.md` |
| FAIL loop / Gate 6 | Orchestrator | `orchestrator.md` | Loop policy + Gate 6 block above |

## Copy and adapt (Jarvis → target)

1. Scaffold orchestration pack and agent contracts ([`README.md`](./README.md) § Copy and adapt).
2. Create or update target orchestration guide (e.g. `docs/ORCHESTRATED_DEVELOPMENT.md`).
3. Paste [`handoff-prompts.example.md`](../templates/universal-agents/handoff-prompts.example.md) into § **Next Agent Directive** (and optional § **Prompt quickstart**).
4. Replace `REPLACE_WITH_*` paths (commands doc, checklist, INDEX read-set pointer).
5. Paraphrase loop addendum from target loop policy (initialized from [`loops-and-rework.md`](../orchestration/loops-and-rework.md)).
6. Confirm `orchestrator.md` rule references the guide section — **no** full template in the contract file.
7. Verify **IND-08**: no Jarvis paths in pasted prompts.
8. Record `PROJ-AGENT-*` evidence in target backlog when scaffolding agents.

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- **Duplicating** the full directive template inside each role contract or INDEX (single source in orchestration guide).
- Setting **`NEW SESSION: NO`** as the default for stage agents without a documented team exception.
- **Inventing** validation commands in directives before `docs/stack/commands.md` exists.
- **Omitting** loop addenda on remediation handoffs when `validation-report.md` has **FAIL**.
- **Skipping** Gate 6 evidence block structure for code-changing runs.

Routine paste of example blocks, placeholder replacement, and cross-links from orchestrator contract do **not** require extra approval.

## Decisions recorded for `JR-AGENT-004`

| Topic | Default | Notes |
| --- | --- | --- |
| Canonical platform doc | **`docs/universal-agents/handoff-prompts.md`** | This file |
| Target publication | **Orchestration guide § Next Agent Directive** | Paste from `handoff-prompts.example.md` |
| Role contracts | **Pointer only** | Full template deferred here (`JR-AGENT-002`); avoids WFD-style duplication |
| Base template fields | Role, session, folder, manifest, tier, objective, contract check, inputs, commands, outputs, manifest rule, stops | Aligned with [`task-folder-model.md`](../orchestration/task-folder-model.md) |
| `NEW SESSION` | **YES** for stage agents | Fresh-context default |
| Loop handoffs | **Addendum block** | Required fields per [`loops-and-rework.md`](../orchestration/loops-and-rework.md) |
| Gate 6 | **Separate evidence block** | Not a Next Agent Directive to Planner/Builder/Tester/Validator |
| Commands | **Verified manifests only** | Same rule as contracts and read sets |
| Read sets in directives | **Reference** INDEX or guide appendix | Tables live under `JR-AGENT-003` |
| Jarvis paths in targets | **Forbidden** | **IND-08** |

## Related material

| Resource | Role |
| --- | --- |
| [`../templates/universal-agents/handoff-prompts.example.md`](../templates/universal-agents/handoff-prompts.example.md) | Paste-ready target section |
| [`../templates/universal-agents/orchestrator.example.md`](../templates/universal-agents/orchestrator.example.md) | Handoff rule → orchestration guide |
| [`minimum-read-sets.md`](./minimum-read-sets.md) | Per-role read order |
| [`../orchestration/README.md`](../orchestration/README.md) | Orchestration pack index |
| [`../roadmap/backlog.md`](../roadmap/backlog.md) | `JR-AGENT-004` |
