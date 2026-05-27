# Target self-containment — validation vocabulary (`JR-VALIDATION-004`)

Canonical **validation layer** for **VAL-CAT-06** (Independence & handoff): how agents map operational checklist rows to evidence tiers, family rollups, and `PROJ-HANDOFF-*` backlog bullets. Use with category scoping in [`categories.md`](./categories.md) (`JR-VALIDATION-001`) and init evidence in [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) (`JR-VALIDATION-002`) or [`runnable-init-evidence.md`](./runnable-init-evidence.md) (`JR-VALIDATION-003`).

**Operational checklist (targets):** `docs/handoff-self-containment.md` — copied from [`../templates/universal-handoff/handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md); scaffold guide [`../universal-handoff/README.md`](../universal-handoff/README.md) (`JR-UNIVERSAL-007`).  
**Orchestration slice:** **ORCH-IND-01–10** in the same target file — copy rules [`../orchestration/self-containment.md`](../orchestration/self-containment.md) (`JR-ORCH-007`).  
**Jarvis completion claims:** [`completion-claims.md`](./completion-claims.md) (`JR-VALIDATION-005`).

## Design goals

| Goal | How this document helps |
| --- | --- |
| **Two layers, one audit** | Operational rows live in the target checklist file; validation vocabulary stays in Jarvis `docs/validation/` |
| **Agent efficiency** | Seven **VAL-IND-FAM-*** families roll up **IND-*** / **ORCH-IND-*** without redefining rows |
| **Readable audits** | `PROJ-HANDOFF-*` evidence cites `VAL-CAT-06` + `VAL-EVID-02/03/04/05` + optional FAM summary |
| **No tier sprawl** | **VAL-CAT-06** uses existing tiers **02–05** only — no **VAL-EVID-08** |

---

## Two layers (operational vs validation)

| Layer | Where it lives | What it contains | Who edits after copy |
| --- | --- | --- | --- |
| **Operational** | Target `docs/handoff-self-containment.md` | **IND-01**–**IND-17** pass/fail/N/A; **ORCH-IND-01**–**10** when orchestration exists; suggested `rg` scans | Target team — bump checklist version when rows change |
| **Validation** | This file (Jarvis `docs/validation/`) | **VAL-IND-FAM-*** families, tier minimums, rollup format, init/change triggers, pause points | Jarvis maintainers only |

**Rule:** Agents **execute** operational rows in the target repo and **record** validation evidence on `PROJ-HANDOFF-*` using tiers from [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) / [`runnable-init-evidence.md`](./runnable-init-evidence.md). Do not duplicate **IND-*** prose into a second target checklist file.

**Distinction from other vocabularies:**

| Concept | ID family | Answers |
| --- | --- | --- |
| Validation category | **VAL-CAT-06** | Is the **project** independent of Jarvis? |
| Independence families (rollup) | **VAL-IND-FAM-01**–**07** | Which **slice** of independence was audited? |
| Operational rows | **IND-***, **ORCH-IND-*** | Itemized pass/fail in target checklist |
| Handoff backlog tasks | **PROJ-HANDOFF-*** | Are handoff gates closed with evidence? |
| Merge-ready (change) | **MG-01**–**MG-05** | May this **change** merge? — **not** init handoff |

---

## Evidence tiers for VAL-CAT-06

**Do not** introduce **VAL-EVID-08** or other new tiers for self-containment. **VAL-CAT-06** minimums match doc-only and runnable init tables in `002` / `003`:

| Tier | ID | What it proves for independence | Maps to operational work |
| --- | --- | --- | --- |
| **2 — Mechanical** | `VAL-EVID-02` | Forbidden patterns absent; scans clean or explained | Template § Suggested scans; **IND-01**, **IND-02**, **IND-04**, **ORCH-IND-01**–**03** |
| **3 — Contract** | `VAL-EVID-03` | Each required **IND-*** / **ORCH-IND-*** row marked pass/fail/N/A with date | Full target `handoff-self-containment.md` walk |
| **4 — Consistency** | `VAL-EVID-04` | README ↔ backlog ↔ ADRs agree on boundaries and stack | **IND-17**; README sync per [`readme-sync.md`](../target-roadmap/readme-sync.md) |
| **5 — Human** | `VAL-EVID-05` | User accepted residual risk or partial handoff | `PROJ-HANDOFF-003` only |

**Init minimum:** **T2 + T3 + T4** before claiming **VAL-CAT-06 PASS**. **T5** is always required for Layer 3 handoff (`PROJ-HANDOFF-003`), not a substitute for **T2–T4**.

**Runnable init:** Same **VAL-CAT-06** tiers — execution tiers **VAL-EVID-06** / **07** do not replace independence evidence.

---

## Independence families (`VAL-IND-FAM-*`)

Families group operational rows for rollup summaries and spot re-audits. A family **passes** when every **required** row in its mapping is **pass** or documented **N/A** (orchestration family **N/A** when `.cursor/orchestrations/` does not exist).

| Family | ID | Operational rows | Theme |
| --- | --- | --- | --- |
| **Links & platform IDs** | **VAL-IND-FAM-01** | **IND-01**, **IND-02**, **IND-03** | No Jarvis URLs or **JR-*** as required reading |
| **Paths & workspace** | **VAL-IND-FAM-02** | **IND-04**, **IND-05**, **IND-06** | No escape paths; target-local paths only |
| **Rules & agents** | **VAL-IND-FAM-03** | **IND-07**, **IND-08**, **IND-09** | `.cursor/rules` and agent contracts resolve in-repo |
| **Documentation map** | **VAL-IND-FAM-04** | **IND-10**, **IND-11**, **IND-12** | README § Documentation honest; no copy-only stubs |
| **Tooling & packages** | **VAL-IND-FAM-05** | **IND-13**, **IND-14**, **IND-15** | Manifests and CI independent of scaffold repo |
| **Backlog honesty** | **VAL-IND-FAM-06** | **IND-16**, **IND-17** | No chat-only promises; README/ADR/backlog aligned |
| **Orchestration** | **VAL-IND-FAM-07** | **ORCH-IND-01**–**ORCH-IND-10** | Target orchestration pack self-contained — **N/A** if no `.cursor/orchestrations/` |

### Mapping to `PROJ-HANDOFF-*` (typical split)

| Backlog task | Primary families | Typical operational rows |
| --- | --- | --- |
| `PROJ-HANDOFF-001` | **FAM-01**, **FAM-02**, **FAM-04** (partial) | **IND-01**–**IND-06**, **IND-10**–**IND-12** |
| `PROJ-HANDOFF-002` | **FAM-03**, **FAM-05**, **FAM-06** (partial) | **IND-07**–**IND-09**, **IND-13**–**IND-17** |
| `PROJ-HANDOFF-003` | _(human only)_ | **VAL-EVID-05** — quote user consent; does not replace **IND-*** pass |

When a single task owns the whole audit, cite all families that passed in one rollup (see [Evidence rollup](#evidence-rollup-for-proj-handoff)).

---

## Evidence rollup for `PROJ-HANDOFF-*`

Extend [`conventions.md`](../target-roadmap/conventions.md#evidence) and init evidence formats with **VAL-CAT-06** + optional **FAM** shorthand.

### Canonical pattern

```markdown
- [x] `PROJ-HANDOFF-001`: Confirm no generated doc links to the Jarvis repository. **required for handoff**
  - Evidence: VAL-CAT-06 PASS — VAL-EVID-02/03: FAM-01,02,04 pass 2026-05-27; IND-01–IND-06, IND-10–IND-12 pass; `rg -i jarvis` clean outside docs/roadmap provenance.
```

| Segment | Required | Rules |
| --- | --- | --- |
| `VAL-CAT-06` | Yes | Always **06** for independence evidence |
| `PASS` / `FAIL` / `PARTIAL` | Yes | `PARTIAL` only with **VAL-EVID-05** + [`handoff.md`](../target-roadmap/handoff.md#partial-handoff-exception) |
| `VAL-EVID-02/03/04` | Yes | Lowest tiers that satisfy init minimum; omit **05** on **001**/**002** — **05** belongs on **003** |
| `FAM-0N` | Optional | Comma-separated family numbers when rollup helps cold resume |
| Body | Yes | Date; **IND-** range or family list; scan one-liner — no “looks independent” |

### Split across handoff tasks

```markdown
- [x] `PROJ-HANDOFF-001`: Confirm no generated doc links to the Jarvis repository. **required for handoff**
  - Evidence: VAL-CAT-06 PASS — VAL-EVID-02/03: FAM-01,02 pass; IND-01–IND-06 pass 2026-05-27.

- [x] `PROJ-HANDOFF-002`: Confirm rules and agents cite only target-project paths. **required for handoff**
  - Evidence: VAL-CAT-06 PASS — VAL-EVID-03: FAM-03,05 pass; IND-07–IND-15 pass; sampled `.cursor/rules/index.md`.

- [x] `PROJ-HANDOFF-003`: User acknowledges initialization handoff. **required for handoff**
  - Evidence: VAL-EVID-05: user sign-off 2026-05-27; residual: deferred PROJ-STACK-001 execution.
  - Evidence: VAL-CAT-06 PASS — VAL-EVID-04: IND-17 / readme-sync pass; FAM-06 pass.
```

### Orchestration addendum (when `PROJ-ORCH-*` closed)

```markdown
  - Evidence: VAL-CAT-06 PASS — VAL-EVID-03: FAM-07 pass; ORCH-IND-01–10 pass 2026-05-27 (`.cursor/orchestrations/`).
```

### Partial handoff

```markdown
  - Evidence: VAL-CAT-06 PARTIAL — VAL-EVID-02/03/05: FAM-01 fail IND-01 (monorepo README link retained per user); user approved partial handoff 2026-05-27 on PROJ-HANDOFF-003.
```

Do **not** claim **VAL-CAT-06 PASS** on **001**/**002** when any **required** **IND-*** or **ORCH-IND-*** row is **fail** unless **PARTIAL** + **VAL-EVID-05** is recorded on **003**.

---

## Agent workflow

### Initialization (run VAL-CAT-06 last)

After other required categories per init path ([`categories.md` § Initialization path](./categories.md#initialization-path--required-categories)):

1. Confirm target `docs/handoff-self-containment.md` exists (**VAL-EVID-01** on **VAL-CAT-01** if scaffolded this session).
2. Run template § **Suggested scans** → **VAL-EVID-02** (mechanical).
3. Walk each **IND-*** row (and **ORCH-IND-*** if orchestration scaffolded) → mark pass/fail/N/A in the **target** file → **VAL-EVID-03**.
4. If README changed late, run readme-sync cross-walk → **VAL-EVID-04** (**IND-17**, **FAM-06**).
5. Record **VAL-CAT-06** rollup on `PROJ-HANDOFF-001` / `002` (and **07** line on **003** when splitting consistency + human).
6. Complete **VAL-EVID-05** via `PROJ-HANDOFF-003`.
7. Claim initialization handoff only per [`handoff.md`](../target-roadmap/handoff.md).

**Order rule:** Run **VAL-CAT-06** **after** **VAL-CAT-02**–**05** (and **07** when runnable) so link fixes and README edits are stable before the final independence pass.

### Change phase — spot re-audit triggers

Full **VAL-CAT-06** is **not** required on every feature PR. Re-run operational rows (and update evidence) when the change **may** reintroduce external dependencies:

| Trigger | Minimum re-audit |
| --- | --- |
| New or bulk-copied `.cursor/rules`, agents, or orchestration templates | **FAM-03**, **FAM-07**; all touched **IND-07**–**IND-09**, **ORCH-IND-*** |
| README § Documentation or `docs/` link sweep | **FAM-01**, **FAM-04**; **IND-01**, **IND-10**–**IND-12** |
| `package.json` / CI / deploy config adds external repo fetch | **FAM-05**; **IND-13**–**IND-15** |
| ADR or rule cites scaffold repo URL | **FAM-01**, **FAM-03**; fix then **IND-01**, **IND-07** |
| User reports agents “need Jarvis” | **FAM-04** first — broken target-local links before assuming missing platform |
| Post-handoff orchestration enablement | Full **ORCH-IND-01**–**10** per [`self-containment.md`](../orchestration/self-containment.md) |

Record spot re-audit on the **change** `PROJ-*` or PR evidence as `VAL-CAT-06 PASS — VAL-EVID-02/03: spot re-audit FAM-03 2026-05-27; IND-07–IND-09 pass` — do not reopen **PROJ-HANDOFF-001** unless the user requests full handoff re-verification.

### Suggested commands (target repo root)

Same as template — customize scaffold name if not Jarvis:

```bash
rg -i 'jarvis' --glob '!node_modules' --glob '!.git' docs adrs .cursor .github README.md 2>/dev/null || true
rg '\.\./.*jarvis|Projects/jarvis' --glob '!node_modules' --glob '!.git' 2>/dev/null || true
rg 'JR-[A-Z]+-[0-9]+' docs adrs .cursor .github README.md 2>/dev/null || true
rg -i 'jarvis|REPLACE_WITH_|\.\./.*orchestration' .cursor/orchestrations .cursor/agents 2>/dev/null || true
```

Review hits: historical mentions in `docs/roadmap/` may be acceptable; **required** links to Jarvis are not.

---

## Human input (pause points)

Jarvis must **stop and ask** before:

| Situation | Why |
| --- | --- |
| Marking **VAL-CAT-06 PASS** while **IND-*** or **ORCH-IND-*** rows are **fail** | Misstates independence — use **PARTIAL** + **003** |
| Skipping **VAL-CAT-06** because the project sits beside Jarvis in a workspace | Partial-handoff exception per [`handoff.md`](../target-roadmap/handoff.md) |
| Inventing **VAL-EVID-08** or new tier IDs for scans | Breaks alignment with `002` / `003` |
| Duplicating **IND-*** checklist into `docs/validation-self-containment.md` in the target | Violates single operational file ([`self-containment.md`](../orchestration/self-containment.md)) |
| Claiming init handoff complete without walking target `handoff-self-containment.md` | **VAL-EVID-03** requires row-level pass/fail |
| Replacing **VAL-IND-FAM-*** or **VAL-EVID-*** IDs in closed `PROJ-*` evidence | Breaks audit trail |

**Defaults without asking** (notify user):

- Operational checklist = target `docs/handoff-self-containment.md` only
- **VAL-CAT-06** at init uses **VAL-EVID-02/03/04** + **05** on **003**
- Optional **FAM-** shorthand in rollup bullets
- Change-phase: spot re-audit only when triggers apply

---

## Decisions recorded for `JR-VALIDATION-004`

| Topic | Decision |
| --- | --- |
| Canonical doc | This file — `self-containment-checks.md` |
| Two layers | Operational **IND-*** / **ORCH-IND-*** in target; validation vocabulary in Jarvis |
| Evidence tiers for **VAL-CAT-06** | **VAL-EVID-02**, **03**, **04**, **05** only — **no VAL-EVID-08** |
| Family IDs | **VAL-IND-FAM-01**–**07** map to **IND-01**–**17** and **ORCH-IND-01**–**10** |
| **PROJ-HANDOFF-*** rollup | `VAL-CAT-06` + tiers + optional `FAM-0N` + IND range / scan summary |
| Init order | **VAL-CAT-06** last among required categories |
| Change phase | Spot re-audit on triggers — not full handoff on every PR |
| Operational doc sprawl | **No** second target self-containment checklist file |
| WFD | Discipline only — no WFD product or gap identifiers |
| Completion claims | [`completion-claims.md`](./completion-claims.md) (`JR-VALIDATION-005`) |

## Related

| Doc | Role |
| --- | --- |
| [`README.md`](./README.md) | Validation pack index |
| [`categories.md`](./categories.md) | **VAL-CAT-06** definition |
| [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) | Init tier **02–05** |
| [`runnable-init-evidence.md`](./runnable-init-evidence.md) | Runnable init; **06** last unchanged |
| [`../templates/universal-handoff/handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md) | **IND-*** / **ORCH-IND-*** template |
| [`../universal-handoff/README.md`](../universal-handoff/README.md) | Copy workflow (`JR-UNIVERSAL-007`) |
| [`../orchestration/self-containment.md`](../orchestration/self-containment.md) | **ORCH-IND** copy-time rules |
| [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | Three layers + partial handoff |
| [`completion-claims.md`](./completion-claims.md) | **VAL-CLAIM-03**, **10** — handoff vs human approval (`JR-VALIDATION-005`) |
