# Next Agent Directive — paste into target orchestration guide

> **Template** — Copy the sections below into the target orchestration guide (for example `docs/ORCHESTRATED_DEVELOPMENT.md`). Replace `REPLACE_WITH_*` placeholders. **Delete this callout** in the target file. Do not leave Jarvis platform IDs or Jarvis repository URLs in target artifacts.

Orchestrator handoffs use these blocks so the next role can run in a **fresh session** without chat memory. Role contracts (`.cursor/agents/*.md`) point here — they do **not** duplicate the full template.

**Read sets:** `REPLACE_WITH_MINIMUM_READ_SETS_PATH` (default: `.cursor/agents/INDEX.md` § Minimum read sets).

**Loop policy:** Paraphrase remediation and human rework routing into this guide (do not link to Jarvis platform docs in the target file).

---

## Next Agent Directive

Every Orchestrator handoff to Planner, Builder, Tester, or Validator MUST include a completed block in this shape.

### Base template

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
<Path A | B | C | none> — <skipped stages from manifest>

OBJECTIVE:
- <success outcome from manifest objective>

CONTRACT CHECK:
- Controlling artifacts: <list>
- Scope allowlist: <files/folders>
- Exit criteria: <role output + behavioral checks>
- Locked artifacts: <from manifest or "none">

REQUIRED INPUTS (read before acting):
- .cursor/orchestrations/<task-id>/task-manifest.json
- <role-specific artifacts>
- Minimum read set: REPLACE_WITH_MINIMUM_READ_SETS_PATH

VALIDATION COMMANDS (verified only):
- <from REPLACE_WITH_COMMANDS_DOC or README § Development>
- If none: "none" — <reason>

OUTPUTS (this session):
- <role-owned artifacts>

MANIFEST RULE:
Do not edit task-manifest.json — Orchestrator owns routing fields.

STOP CONDITIONS:
- Objective or ACs are contradictory
- Work requires files outside scope allowlist
- Accepted ADR conflict
- Required command missing, unverified, or impossible
- Dependency or toolchain change without explicit human approval
- Output contract cannot be met — report to Orchestrator
```

### Planner — fill hints

- **Controlling artifacts:** manifest only.
- **Outputs:** `plan.md`, `acceptance-criteria.md`; `test-matrix.md` when medium/large or testable code.
- **Inputs:** `adrs/INDEX.md`, `adrs/GOVERNANCE.md`; cited Accepted ADRs; `REPLACE_WITH_VALIDATION_CHECKLIST_PATH` — applicable sections; `REPLACE_WITH_COMMANDS_DOC`.

### Builder — fill hints

- **Controlling artifacts:** `plan.md`, `acceptance-criteria.md`.
- **Outputs:** `build-log.md` + code per plan file map.
- **Remediation loop:** add prior `validation-report.md` § Required remediations; append build log.

### Tester — fill hints

- **Controlling artifacts:** `acceptance-criteria.md`, `build-log.md`.
- **Outputs:** `test-report.md`; update `test-matrix.md` actual coverage when required.

### Validator — fill hints

- **Controlling artifacts:** plan, ACs, build log, test report.
- **Outputs:** `validation-report.md` with verdict PASS | PASS_WITH_NOTES | FAIL.
- **Inputs:** checklist applicable sections; `REPLACE_WITH_VALIDATION_CHECKLIST_PATH`; workflow-gates rule for **MG-***.

### Loop and rework addendum

Append when routing after Validator **FAIL** or human `rejected_rework`:

```text
LOOP CONTEXT:
- Type: <validation remediation | human rework>
- loop_count: <n> / max_loops: <m>
- rework_count: <n> (if applicable)
- Read: validation-report.md § Required remediations OR human-approval.md § Rework directive

SCOPE:
- Do not expand beyond allowlist + remediations
- Builder: append build-log.md
- Tester/Validator: replace prior stage reports for this invocation
```

### Gate 6 — human evidence (Orchestrator → human)

Not pasted to stage agents. Use when `status` is `awaiting_human`:

```text
GATE 6 — HUMAN APPROVAL REQUEST

TASK: <task-id> — <objective>
RISK / SKIPS: <tier> — <path / skipped_stages>

EVIDENCE:
- Files changed: <build-log.md>
- AC coverage: <test-report.md if present>
- Validation: <validation-report.md verdict or small-run substitute>
- Commands run / not run: <artifacts + command_evidence>

MERGE-READY (MG-*): <status per workflow-gates rule>

REQUEST: Approve | Approve with conditions | Reject | Reject — rework (Builder | Planner)
```

Record in `human-approval.md` and manifest `human_approval`.

---

## Prompt quickstart (optional)

| Goal | Role | Contract | First outputs |
| --- | --- | --- | --- |
| New run | Orchestrator | `.cursor/agents/orchestrator.md` | manifest + directive |
| Plan | Planner | `planner.md` | plan + ACs (+ matrix) |
| Implement | Builder | `builder.md` | build-log |
| Test | Tester | `tester.md` | test-report |
| Audit | Validator | `validator.md` | validation-report |
| FAIL / Gate 6 | Orchestrator | `orchestrator.md` | loop addendum or Gate 6 block |
