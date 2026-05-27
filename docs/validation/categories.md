# Generic validation categories (`JR-VALIDATION-001`)

Canonical **validation categories** for target projects after Jarvis initialization. Use categories to **scope audits** (what to look at); use the target [`validation-checklist.md`](../templates/universal-validation/validation-checklist.md) for **checkable rows** (**MG-***, **TST-***, extensions).

**Prerequisites:** [`../orchestration/init-paths.md`](../orchestration/init-paths.md), [`../orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md)  
**Evidence detail:** [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) (`JR-VALIDATION-002`); [`runnable-init-evidence.md`](./runnable-init-evidence.md) (`JR-VALIDATION-003`); [`self-containment-checks.md`](./self-containment-checks.md) (`JR-VALIDATION-004`); [`completion-claims.md`](./completion-claims.md) (`JR-VALIDATION-005`)

## Design goals

| Goal | How categories help |
| --- | --- |
| **Agent efficiency** | Cold sessions pick a category (or phase) instead of re-reading the entire checklist |
| **Readable audits** | Humans see “architecture alignment” vs a flat list of 40 checkboxes |
| **Resume-friendly** | `PROJ-*` tasks and handoff evidence name **VAL-CAT-*** in `Evidence:` bullets |
| **No vocabulary collision** | Categories ≠ lifecycle gates ≠ **MG-*** ≠ **PROJ-HANDOFF-*** |

## Two validation phases

| Phase | Code | When | Primary question |
| --- | --- | --- | --- |
| **Initialization** | `VAL-PHASE-INIT` | End of Jarvis scaffold / before `PROJ-HANDOFF-*` | Did we create a **self-contained foundation** matching the chosen init path? |
| **Change** | `VAL-PHASE-CHANGE` | Each PR, feature batch, or orchestrated run | Does this **change** respect architecture, tests, tooling, and merge-ready policy? |

**Rule:** Do not run **change-phase** merge-ready (**MG-***) audits to close **initialization** handoff unless the init work itself was executed as an orchestrated run with those artifacts. Initialization uses **VAL-CAT-*** + `docs/handoff-self-containment.md` + required `PROJ-*`.

## Category catalog

| ID | Category | Initialization (`VAL-PHASE-INIT`) | Change (`VAL-PHASE-CHANGE`) |
| --- | --- | --- | --- |
| **VAL-CAT-01** | **Scaffold completeness** | Required — files exist per init path | N/A unless adding new scaffold |
| **VAL-CAT-02** | **README & doc routing** | Required | When README § Documentation or map changes |
| **VAL-CAT-03** | **Architecture & ADR alignment** | Medium/large required; small when README has non-negotiables | When boundaries, persistence, auth, or ADR-touched code change |
| **VAL-CAT-04** | **Rules & agent workflow** | Per init path (`PROJ-RULE-*`, optional `PROJ-AGENT-*` / `PROJ-ORCH-*`) | When `.cursor/rules/` or agent contracts change |
| **VAL-CAT-05** | **Stack & command fidelity** | When repo has manifests or README claims a stack | When scripts, lockfiles, or `docs/stack/*` change |
| **VAL-CAT-06** | **Independence & handoff** | Required — always last major audit | Re-run after copying orchestration templates or fixing external links |
| **VAL-CAT-07** | **Executable & test evidence** | Only if init produced runnable code/config | Required for production changes; drives **TST-*** and **MG-04** / **MG-05** |
| **VAL-CAT-08** | **Orchestration integrity** | Large init (and medium if `_template/` copied) | When `task-manifest.json` or stage artifacts change |

### VAL-CAT-01 — Scaffold completeness

**Question:** Does the repo contain the **minimum durable files** for the chosen initialization path?

| Init path | Typical evidence |
| --- | --- |
| **Small** | Root README; `docs/roadmap/`; 1–2 rules; `docs/documentation-conventions.md` (or equivalent); **minimal** `docs/validation-checklist.md`; `docs/handoff-self-containment.md` |
| **Medium** | + `adrs/` index/governance; PR guide; stack docs when applicable; fuller checklist |
| **Large** | + agent INDEX/contracts; orchestration `_template/` + guide; `workflow-gates.mdc` when used |

**Maps to:** `PROJ-README-*`, `PROJ-DOC-*`, `PROJ-ADR-*`, `PROJ-AGENT-*`, `PROJ-ORCH-*` per [`init-paths.md`](../orchestration/init-paths.md#a3-proj--scope-matrix-by-init-path).

**Not:** Passing **MG-05** (tooling) — that is **VAL-CAT-07** when code exists.

### VAL-CAT-02 — README & doc routing

**Question:** Can a cold agent find the right docs from the README alone?

| Check | Evidence examples |
| --- | --- |
| Required outline sections present | [`outline.md`](../target-readme/outline.md) audit |
| Documentation map links resolve | `rg` / file existence; open `PROJ-DOC-*` for gaps |
| No Jarvis URLs in required reading paths | Part of **VAL-CAT-06** |
| README § Development matches `docs/stack/commands.md` when stack exists | Cross-link **VAL-CAT-05** |

**Maps to:** `PROJ-README-*`, `PROJ-DOC-*`; Layer 1 in [`handoff-checklist.md`](../target-readme/handoff-checklist.md).

### VAL-CAT-03 — Architecture & ADR alignment

**Question:** Are durable decisions recorded and consistent with the README?

| Check | Evidence examples |
| --- | --- |
| `adrs/INDEX.md` + `GOVERNANCE.md` exist when path requires | File paths |
| Each README non-negotiable has **Accepted** ADR or open `PROJ-ADR-*` | ADR status in index |
| Rules cite **Accepted** ADRs, not Jarvis paths | Sample rule `see` links |
| Known drift documented | Target alignment-gaps doc per governance |

**Maps to:** `PROJ-ADR-*`; checklist **MG-01**, **MG-02** at **change** phase; ADR annex rows when present.

### VAL-CAT-04 — Rules & agent workflow

**Question:** Are agent constraints discoverable and activation-correct?

| Check | Evidence examples |
| --- | --- |
| `.cursor/rules/index.md` lists rules with activation mode | [`activation-modes.md`](../universal-rules/activation-modes.md) |
| Always-apply vs globs match project risk | No doc-syntax rule until `JR-RULE-004` closes |
| Large path: agent INDEX + five contracts | Paths under `.cursor/agents/` |
| Large path: orchestration guide + `workflow-gates.mdc` | Target-owned names only |

**Maps to:** `PROJ-RULE-*`, `PROJ-AGENT-*`, `PROJ-ORCH-*`.

### VAL-CAT-05 — Stack & command fidelity

**Question:** Do documented commands and stack facts match the repository?

| Check | Evidence examples |
| --- | --- |
| `docs/stack/stack-profile.md` matches detection + user confirmation | [`confirmation.md`](../stack-scaffolding/confirmation.md) |
| `docs/stack/commands.md` and README § Development list **verified** scripts only | `package.json` / manifest excerpt in evidence |
| Testing/runtime docs reference same command names | [`commands.md`](../stack-scaffolding/commands.md) |
| No invented package manager or test runner | Fail category if scripts missing |

**Maps to:** `PROJ-STACK-*`; checklist **TOOL-*** extension rows.

### VAL-CAT-06 — Independence & handoff

**Question:** Can the target operate without Jarvis?

**Evidence detail:** [`self-containment-checks.md`](./self-containment-checks.md) (`JR-VALIDATION-004`) — **VAL-IND-FAM-01**–**07**, **VAL-EVID-02/03/04/05** for **06**; operational **IND-*** / **ORCH-IND-*** in target `docs/handoff-self-containment.md`.

| Check | Evidence examples |
| --- | --- |
| `docs/handoff-self-containment.md` audit complete | Itemized pass/fail per **IND-*** / **ORCH-IND-*** |
| `PROJ-HANDOFF-001` / `PROJ-HANDOFF-002` with `Evidence:` | `VAL-CAT-06` + tiers + optional **FAM-** rollup — [`conventions.md`](../target-roadmap/conventions.md) |
| User sign-off for `PROJ-HANDOFF-003` when required | **VAL-EVID-05** |
| Orchestration copied: **ORCH-IND-01–10** | [`self-containment.md`](../orchestration/self-containment.md); **VAL-IND-FAM-07** |

**Maps to:** Handoff vocabulary in [`gates-and-checks.md`](../orchestration/gates-and-checks.md) — **not** lifecycle Gate 5 or **MG-03**.

**Distinction:**

| Concept | ID family | Answers |
| --- | --- | --- |
| Category (this doc) | **VAL-CAT-06** | Is the **project** independent of Jarvis? |
| Handoff backlog tasks | **PROJ-HANDOFF-*** | Are handoff gates closed with evidence? |
| Merge-ready (change) | **MG-01**–**MG-05** | May this **change** merge per policy? |

### VAL-CAT-07 — Executable & test evidence

**Question:** Do claims about runnable behavior have command output or named manual steps?

| When | Expectation |
| --- | --- |
| **Doc-only init** ([`doc-only-init-evidence.md`](./doc-only-init-evidence.md)) | **VAL-EVID-00** — mandatory N/A line; no **MG-*** / test commands at init |
| **Init with generated config/tooling** ([`runnable-init-evidence.md`](./runnable-init-evidence.md)) | **VAL-EVID-06** manifest trace + **VAL-EVID-07** default quality chain (exit codes) |
| **Every production change** | **TST-*** + **MG-04** / **MG-05** in checklist; `test-report.md` when Tester ran |

**Maps to:** Checklist § Tests and evidence, § Merge-ready gates; Tester/Validator artifacts.

### VAL-CAT-08 — Orchestration integrity

**Question:** Can a fresh Orchestrator resume a run from the task folder alone?

| Check | Evidence examples |
| --- | --- |
| `_template/` matches [`task-manifest.example.json`](../templates/orchestration/_template/task-manifest.example.json) shape | Schema fields present |
| `task_id` equals folder slug | One example folder or dry-run note |
| Stage templates list owners per [`artifact-ownership.md`](../orchestration/artifact-ownership.md) | File headers |
| Checklist orchestration appendix filled or explicitly deferred | `validation-checklist.md` § Orchestration |

**Maps to:** `PROJ-ORCH-*`; lifecycle gates 0–6 at **change** phase only.

## Initialization path → required categories

Use at handoff. **Required** = must pass or be explicitly **N/A** with one-line justification in `PROJ-HANDOFF-*` evidence.

| Category | Small | Medium | Large |
| --- | --- | --- | --- |
| **VAL-CAT-01** Scaffold | Required | Required | Required |
| **VAL-CAT-02** README & routing | Required | Required | Required |
| **VAL-CAT-03** ADR alignment | If README non-negotiables | Required | Required |
| **VAL-CAT-04** Rules & agents | Rules only | Rules + optional agent INDEX | Rules + agents + orch guide |
| **VAL-CAT-05** Stack fidelity | If stack claimed | If stack claimed | Required when tooling exists |
| **VAL-CAT-06** Independence | Required | Required | Required |
| **VAL-CAT-07** Executable | N/A unless code/config generated | As applicable | As applicable |
| **VAL-CAT-08** Orchestration | N/A | N/A unless `_template/` copied | Required |

## Category → checklist → gates (change phase)

When auditing a **change**, map categories to checklist sections and lifecycle stages:

| Category | Checklist section(s) | Typical lifecycle touchpoint |
| --- | --- | --- |
| **VAL-CAT-03** | **MG-01**, **MG-02**, ADR annexes | Gate 2 plan, Gate 5 validation |
| **VAL-CAT-04** | Topic rules (implicit); orchestration appendix | Gates 0–2 |
| **VAL-CAT-05** | **TOOL-*** extensions | Gate 5 |
| **VAL-CAT-07** | **TST-***, **MG-04**, **MG-05** | Gates 4–5 |
| **VAL-CAT-08** | Orchestration appendix; manifest `gate_status` | Gates 0–6 |

Full vocabulary rules: [`gates-and-checks.md`](../orchestration/gates-and-checks.md).

## Category → `PROJ-*` prefixes (initialization)

| `PROJ-*` prefix | Primary categories |
| --- | --- |
| `PROJ-README-*` | **VAL-CAT-02** |
| `PROJ-ADR-*` | **VAL-CAT-03** |
| `PROJ-RULE-*` | **VAL-CAT-04** |
| `PROJ-DOC-*` | **VAL-CAT-01**, **VAL-CAT-02** |
| `PROJ-STACK-*` | **VAL-CAT-05**, **VAL-CAT-07** (when runnable) |
| `PROJ-AGENT-*` | **VAL-CAT-04** |
| `PROJ-ORCH-*` | **VAL-CAT-04**, **VAL-CAT-08** |
| `PROJ-HANDOFF-*` | **VAL-CAT-06** (+ backlog honesty) |

## How agents use categories (workflow)

### Initialization session (Jarvis)

1. Read target init path from intake or backlog.
2. List **required** categories from the matrix above.
3. For each category, gather evidence (paths, command summaries, scan results).
4. Record in `PROJ-*` `Evidence:` bullets per [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) (doc-only) or [`runnable-init-evidence.md`](./runnable-init-evidence.md) (runnable): `VAL-CAT-0N PASS|N/A|DEFERRED — VAL-EVID-0N: …`.
5. Run **VAL-CAT-06** last; then Layer 3 user sign-off per [`handoff.md`](../target-roadmap/handoff.md).

### Change session (Validator / human PR)

1. Determine **touch domains** (same idea as checklist § Applicability).
2. Map domains → **VAL-CAT-03**–**08** as needed.
3. Audit applicable **checklist rows**; write `validation-report.md` → Checklist audit.
4. Do not claim **VAL-CAT-06** satisfied for a feature PR unless the change reintroduced external dependencies.

## Human input (pause points)

| Situation | Ask user |
| --- | --- |
| Small path + README lists hard non-negotiables but user wants zero ADRs | Accept **VAL-CAT-03** deferral with open `PROJ-ADR-*` and documented risk, or require minimal Accepted ADR |
| Monorepo: Jarvis links intentional in dev docs | Partial handoff exception; which paths remain allowed |
| Init generated scripts user refuses to run | Whether **VAL-CAT-07** is deferred to post-handoff `PROJ-STACK-*` |
| User wants target-owned `docs/validation-categories.md` | Approve extra doc (large teams only); keep IDs aligned with this catalog |

## Decisions recorded for `JR-VALIDATION-001`

| Topic | Decision |
| --- | --- |
| Eight categories | Enough granularity for agents; avoids checklist-sized sprawl |
| `VAL-CAT-*` not copied to targets by default | Single checklist file stays canonical in target repos |
| **VAL-CAT-06** separate from **MG-*** | Prevents “merge-ready” language at init handoff |
| **VAL-CAT-07** at doc-only init | **VAL-EVID-00** only — see [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) |
| **VAL-CAT-07** at runnable init | **VAL-EVID-06** + **07** (or deferral) — see [`runnable-init-evidence.md`](./runnable-init-evidence.md) |
| Evidence format at init | `Evidence:` cites `VAL-CAT-0N` + `PASS`/`N/A`/`DEFERRED` + `VAL-EVID-0N` + paths/date — doc-only or runnable guides above |
