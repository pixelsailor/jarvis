# Jarvis completion claims vs human approval (`JR-VALIDATION-005`)

Canonical rules for **what Jarvis (and Jarvis-mode agents) may assert as complete** versus what **requires explicit human approval** before closing work, updating backlog checkboxes, or using handoff / merge-ready language.

Use with evidence tiers in [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) (`JR-VALIDATION-002`) and [`runnable-init-evidence.md`](./runnable-init-evidence.md) (`JR-VALIDATION-003`), category scoping in [`categories.md`](./categories.md) (`JR-VALIDATION-001`), self-containment in [`self-containment-checks.md`](./self-containment-checks.md) (`JR-VALIDATION-004`), and gate vocabulary in [`../orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md) (`JR-ORCH-004`).

**Target handoff layers:** [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) — operational “when may Jarvis claim handoff”; this doc adds **claim discipline** and **forbidden phrasing** across all Jarvis work surfaces.

## Design goals

| Goal | How this document helps |
| --- | --- |
| **Agent efficiency** | Stable **VAL-CLAIM-*** surfaces — agents do not re-derive “can I say done?” from chat |
| **Readable audits** | Backlog `Evidence:` and user messages cite claim type + tier/gate, not vague “finished” |
| **Vocabulary safety** | Completion claims ≠ **VAL-CAT-*** pass ≠ **MG-*** ≠ lifecycle **Gate 6** ≠ **PROJ-HANDOFF-*** |
| **Long-term honesty** | Jarvis defaults to **evidence recorded**; humans own acceptance, merge, and binding ADRs |

---

## Four work surfaces (where claims apply)

| Surface | Code | Primary artifacts | Who “Jarvis” is here |
| --- | --- | --- | --- |
| **Platform maintenance** | `VAL-CLAIM-SURF-P` | Jarvis `docs/roadmap/backlog.md` (`JR-*`) | Agent editing the Jarvis repo |
| **Target initialization** | `VAL-CLAIM-SURF-I` | Target `docs/roadmap/backlog.md` (`PROJ-*`), `VAL-CAT-*`, handoff checklist | Agent scaffolding a target before handoff |
| **Orchestrated change run** | `VAL-CLAIM-SURF-O` | `.cursor/orchestrations/{task-id}/`, **MG-***, `gate_status` | Orchestrator / Planner / Builder / Tester / Validator per contracts |
| **Post-handoff target work** | `VAL-CLAIM-SURF-T` | Target backlog, PRs, orchestration in target | Target team — Jarvis **does not** claim completion here unless user re-engages Jarvis |

**Rule:** Pick the surface first, then apply the **VAL-CLAIM-*** row below. Do not use **SURF-I** handoff wording to close **SURF-O** merge-ready or vice versa.

---

## Claim types (`VAL-CLAIM-*`)

Each row is a **class of assertion**. Satisfy **Jarvis may claim** only when every **Evidence / gate** cell is met; otherwise stop at **Human required** or use the **Forbidden** phrasing table.

| ID | Surface(s) | Jarvis may claim (when evidence met) | Human required before claim |
| --- | --- | --- | --- |
| **VAL-CLAIM-01** | **SURF-I** | `VAL-CAT-0N PASS (VAL-PHASE-INIT)` on a `PROJ-*` or handoff summary with tier IDs (**VAL-EVID-02**–**04** minimum per category doc) | Skip a **required** category for the init path; mark **PASS** with open **fail** **IND-*** rows; replace tier IDs in closed evidence |
| **VAL-CLAIM-02** | **SURF-I** | `PROJ-*` `[x]` with `Evidence:` per [`conventions.md`](../target-roadmap/conventions.md) | `Owner: human` tasks (**003**, partial-handoff waivers); `[x]` without `Evidence:` on **required for handoff** rows |
| **VAL-CLAIM-03** | **SURF-I** | Use handoff template: *“Initialization handoff is complete…”* per [`handoff.md`](../target-roadmap/handoff.md) | **`PROJ-HANDOFF-003`** (**VAL-EVID-05**); any open **required for handoff** task; partial handoff without documented user approval |
| **VAL-CLAIM-04** | **SURF-I** | `VAL-CAT-07` **N/A** at doc-only init (fixed phrase per `002`); `VAL-CAT-05` **N/A** when no manifests | User wants handoff with **unverified** README § Development commands; mixed init without split evidence plan |
| **VAL-CLAIM-05** | **SURF-O** | Lifecycle **Gate 0–5** `passed` / `skipped` only via the **owning role** (Orchestrator sets routing; Planner/Builder/Tester/Validator own stage artifacts) | **Gate 6**; `status: complete`; `awaiting_human` before merge-ready path green |
| **VAL-CLAIM-06** | **SURF-O** | Validator records **MG-01**–**MG-05** in `validation-report.md` → **Checklist audit**; verdict **PASS** / **PASS_WITH_NOTES** | Orchestrator or Jarvis saying **merge-ready** without Validator audit; **MG-*** pass at **SURF-I** init to “speed up” handoff |
| **VAL-CLAIM-07** | **SURF-O** + **SURF-I** (tooling) | Commands **recorded** from verified manifests/README; scaffold docs list scripts that exist | **New** packages, lockfile edits, bulk upgrades ([`dependencies.md`](../stack-scaffolding/dependencies.md)); **invented** commands not in repo |
| **VAL-CLAIM-08** | **SURF-P** | `JR-*` `[x]` in same session as completed work with optional evidence sub-bullet | Marking `JR-*` complete without doing the work; reprioritizing or deleting backlog sections |
| **VAL-CLAIM-09** | **SURF-I**, **SURF-O** | ADR file + index row in **Proposed** status; “ready for review” | **Accepted** in ADR body and index; treating **Proposed** as binding for implementation |
| **VAL-CLAIM-10** | **SURF-I** | **Partial handoff** wording only after user accepts gaps — status in `docs/roadmap/README.md` per [`handoff.md`](../target-roadmap/handoff.md) | “Fully initialized” / “all done” with skipped **required for handoff** tasks |

---

## Authority matrix (who may say what)

During **SURF-I**, Jarvis often performs Planner/Builder/Validator-like work in one session. **Role names still bound claims:**

| Assertion | Jarvis (init scaffold) | Validator (init or run) | Orchestrator | Human |
| --- | --- | --- | --- | --- |
| Category / checklist row pass | Yes — cite **VAL-CAT-*** + **VAL-EVID-*** | Yes — in `validation-report.md` when Validator stage runs | No — routes only | Accepts residual risk (**005**) |
| `PROJ-*` complete | Yes — with `Evidence:` | No | No | **Owner: human** rows |
| Initialization handoff complete | Yes — **after 003** | No | No | **003** sign-off |
| Merge-ready / **MG-*** green | **No** at init | Yes — audit only | `awaiting_human` after green path | PR policy may state merge-ready |
| Lifecycle **Gate 6** / run `complete` | No | No | Records approval artifact | Approves or requests rework |
| ADR **Accepted** | No | No | No | Accepts in ADR + index |
| Dependency / lockfile change | No | No | No | Explicit approval |

**After SURF-I handoff (SURF-T):** Jarvis must not mark target `PROJ-*` or claim handoff unless the user explicitly re-engages Jarvis for follow-up scaffold work.

---

## Forbidden and required phrasing

### Do not say (without meeting **Human required**)

| Phrase (or equivalent) | Why | Use instead |
| --- | --- | --- |
| “Merge-ready” / “ready to merge” | **VAL-CLAIM-06** — **MG-*** + change phase | At init: “handoff complete”; at run: cite Validator verdict + **MG-*** |
| “Validation complete” at init | Conflates init categories with **MG-03** | “Required **VAL-CAT-*** pass for init path” |
| “Fully initialized” with open **required for handoff** `PROJ-*` | **VAL-CLAIM-10** | “Partial handoff” + listed IDs, or finish gates |
| “ADR accepted” for **Proposed** only | **VAL-CLAIM-09** | “ADR **Proposed** — awaiting human acceptance” |
| “Tests passed” without command output | **VAL-EVID-06** / **07** | `Evidence: command — excerpt or exit code` |
| “Orchestration complete” / `status: complete` | **Gate 6** | `awaiting_human` + Gate 6 request |
| “No Jarvis dependency” while **IND-*** fail | **VAL-CAT-06** misstatement | List failing **IND-*** / families; fix or partial handoff |

### Preferred claims (copy-friendly)

**Initialization handoff (after layers 1–3):** use the template in [`handoff.md` § Jarvis may claim handoff when](../target-roadmap/handoff.md#jarvis-may-claim-handoff-when).

**Category pass on `PROJ-*`:**

```markdown
Evidence: VAL-CAT-06 PASS (VAL-PHASE-INIT); VAL-EVID-02,03,04; FAM-01,02,04; IND-01–06,10–12 pass 2026-05-27 — docs/handoff-self-containment.md
```

**Orchestrated run (Validator, not Jarvis init):**

```markdown
Verdict: PASS_WITH_NOTES — MG-01–MG-05 audited in validation-report.md; Gate 6 pending.
```

---

## Surface quick reference

### SURF-I — Target initialization

| Question | Answer |
| --- | --- |
| May Jarvis check `PROJ-*` `[x]`? | Yes, when `Evidence:` satisfies conventions and task is not `Owner: human` |
| May Jarvis say handoff complete? | Yes only after **VAL-CLAIM-03** (all layers + **003**) |
| May Jarvis close **MG-*** at init? | **No** — **VAL-CLAIM-06**; doc-only init uses **VAL-CAT-07** N/A per `002` |
| May Jarvis skip **VAL-CAT-06**? | **No** — ask for partial-handoff exception |
| Who owns “optional follow-ups” list? | Jarvis may **notify**; human accepts on **003** |

Full layer rules: [`handoff.md`](../target-roadmap/handoff.md). Category order: **VAL-CAT-06** last among required categories ([`self-containment-checks.md`](./self-containment-checks.md)).

### SURF-O — Orchestrated change run

| Question | Answer |
| --- | --- |
| May Jarvis set `gate_6_human_approval: passed`? | **No** — human only; Orchestrator **records** |
| May Jarvis set `status: complete`? | **No** until **Gate 6** recorded |
| May Builder/Tester say “done”? | **No** merge-ready or run-complete ([`artifact-ownership.md`](../orchestration/artifact-ownership.md)) |
| Small path A without Validator? | **No** full merge-ready; human substitute audit — [`init-paths.md`](../orchestration/init-paths.md) |

### SURF-P — Jarvis platform `JR-*`

| Question | Answer |
| --- | --- |
| May agent mark `JR-VALIDATION-005` `[x]`? | Yes in same session as shipping `completion-claims.md` + cross-links |
| Evidence sub-bullet? | Optional; add when verification steps help the next agent ([`MAINTENANCE.md`](../roadmap/MAINTENANCE.md)) |

### SURF-T — Post-handoff

Jarvis **must not** claim target feature completion, merge-ready, or handoff. Point users to target `docs/roadmap/backlog.md` and orchestration guide.

---

## Mapping to existing vocabularies

| Human approval need | Primary record | Claim type |
| --- | --- | --- |
| Residual init risk / optional work acknowledged | `PROJ-HANDOFF-003` | **VAL-CLAIM-03**, **VAL-EVID-05** |
| Partial handoff | `docs/roadmap/README.md` handoff status | **VAL-CLAIM-10** |
| Orchestrated run closure | `human-approval.md`, manifest `human_approval` | **VAL-CLAIM-05** |
| Merge-ready for a change | `validation-report.md`, **MG-*** | **VAL-CLAIM-06** |
| Binding architecture | ADR index **Accepted** | **VAL-CLAIM-09** |
| Toolchain change | User message + manifest diff | **VAL-CLAIM-07** |
| Validator skip (arch-touching) | Manifest `flags` + user message | **VAL-CLAIM-05** / **06** |

**Do not introduce** a fifth gate vocabulary. Completion claims **reference** lifecycle, **MG-***, **VAL-CAT-***, and **PROJ-HANDOFF-*** — they do not replace them.

---

## Target adoption (optional)

Targets do **not** receive a copy of this file by default (platform vocabulary in Jarvis `docs/validation/`). When copying orchestration or workflow rules:

1. Paraphrase **§ Forbidden and required phrasing** into the target orchestration guide (no Jarvis URLs).
2. Keep **Gate 6**, **MG-***, and Validator ownership aligned with [`workflow-gates.mdc`](../templates/universal-rules/workflow-gates.mdc).
3. Point init handoff at target [`handoff.md`](../target-roadmap/handoff.md) equivalent once the target owns `docs/roadmap/`.

---

## Human input (pause points)

Jarvis must **stop and ask** before:

| Situation | Claim types |
| --- | --- |
| Partial handoff or skipping **required for handoff** `PROJ-*` | **VAL-CLAIM-03**, **10** |
| Marking **VAL-CAT-06 PASS** with failing **IND-*** / **ORCH-IND-*** | **VAL-CLAIM-01** |
| Handoff while README commands were never run | **VAL-CLAIM-04** |
| Skipping Validator on architecture-touching orchestrated work | **VAL-CLAIM-05**, **06** |
| Any dependency or lockfile change | **VAL-CLAIM-07** |
| Setting ADR to **Accepted** | **VAL-CLAIM-09** |
| Renaming **VAL-CLAIM-*** or conflating with **MG-*** / `gate_*` in Accepted ADRs | All |

**Defaults without asking** (notify user):

- Record tier-appropriate `Evidence:` on each **required for handoff** `PROJ-*` Jarvis closes
- Doc-only **VAL-CAT-07** N/A line per `002`
- **Proposed** ADRs only until human accepts
- Gate 6 wording as **request**, not **complete**

---

## Decisions recorded for `JR-VALIDATION-005`

| Topic | Decision |
| --- | --- |
| Canonical doc | This file — `completion-claims.md` |
| Claim surface IDs | **VAL-CLAIM-SURF-P/I/O/T** — four surfaces |
| Claim type IDs | **VAL-CLAIM-01**–**10** — stable; cite in evidence when helpful |
| vs evidence tiers | Tiers prove *how*; claim types bound *who may say what* |
| vs **VAL-CAT-*** | Categories scope audits; **PASS** is **VAL-CLAIM-01** when tiers met |
| vs **MG-*** / Gate 6 | Merge-ready and run closure are **never** init handoff substitutes |
| Target copy | **No** default copy — paraphrase into orchestration guide only |
| Handoff template | Remains in [`handoff.md`](../target-roadmap/handoff.md); this doc adds cross-surface discipline |
| WFD | Discipline only — no WFD product, gap, or stack identifiers |

## Related

| Doc | Role |
| --- | --- |
| [`README.md`](./README.md) | Validation pack index and read order |
| [`categories.md`](./categories.md) | **VAL-CAT-*** catalog |
| [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) | Init tiers **00–05** |
| [`runnable-init-evidence.md`](./runnable-init-evidence.md) | Tiers **06–07** |
| [`self-containment-checks.md`](./self-containment-checks.md) | **VAL-CAT-06** families |
| [`../orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md) | Lifecycle, **MG-***, handoff vocabularies |
| [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | Three layers + handoff template |
| [`../roadmap/MAINTENANCE.md`](../roadmap/MAINTENANCE.md) | `JR-*` checkbox discipline |
