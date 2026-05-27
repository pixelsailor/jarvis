# Documentation-only initialization — evidence expectations (`JR-VALIDATION-002`)

Canonical **evidence tiers** and **audit rules** when Jarvis closes **initialization** (`VAL-PHASE-INIT`) without producing or changing **runnable** application code, tests, or deployable config. Use with category scoping in [`categories.md`](./categories.md) (`JR-VALIDATION-001`) and init path matrices in [`../orchestration/init-paths.md`](../orchestration/init-paths.md) (`JR-ORCH-005`).

**Runnable init evidence:** `JR-VALIDATION-003` (planned) — when init adds or changes manifests, CI, or executable source.  
**Self-containment operations:** [`../templates/universal-handoff/handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md) + **VAL-CAT-06** — `JR-VALIDATION-004` (planned).  
**Jarvis completion claims:** `JR-VALIDATION-005` (planned).

## Design goals

| Goal | How this document helps |
| --- | --- |
| **Agent efficiency** | Five evidence tiers + per-category minimums — no inventing bar per session |
| **Readable audits** | Humans see `VAL-CAT-0N` + `VAL-EVID-0N` in backlog evidence, not chat claims |
| **Resume-friendly** | Cold sessions re-run the same scans; N/A reasons are one-line and stable |
| **Vocabulary safety** | Doc-only init never closes **MG-*** or lifecycle gates — handoff only |

---

## When this doc applies

### Documentation-only initialization (definition)

An init session (or init **phase** of a larger path) is **documentation-only** when **all** of the following hold:

| Criterion | Meaning |
| --- | --- |
| **No runnable delta** | No new or changed source files that compile, bundle, or execute application logic (no `.ts`/`.js`/`.py`/`.go`/`.rs`/`.java` product code, no changed `package.json` scripts added by Jarvis, no new CI workflows that run product builds) |
| **Durable artifacts are docs + agent config** | Markdown under `docs/`, `adrs/`, root `README.md`, `.cursor/rules/`, optional `.cursor/agents/`, copied orchestration `_template/` **without** filling a live run with code changes |
| **Handoff scope is init** | Auditing `VAL-PHASE-INIT`, not a feature PR (`VAL-PHASE-CHANGE`) |

**Still doc-only** when the **target repo already contains** application code from before Jarvis — init evidence covers **scaffold work only**, not re-validating the whole codebase.

**Not doc-only** (use `JR-VALIDATION-003` when published):

- Jarvis adds or edits lockfiles, manifests, CI, service worker, or generated source
- README § Development gains **new** commands Jarvis expects to be run at handoff
- Init includes “wire minimal app shell” or “add default test script” tasks

When **mixed** (mostly docs + one config file), treat the session as **not** doc-only unless the user explicitly approves **split evidence**: doc categories per this doc; **VAL-CAT-05** / **VAL-CAT-07** per `003` for the config slice.

### Relationship to init path (small / medium / large)

Init path sets **which categories are required** ([`categories.md` § Initialization path → required categories](./categories.md#initialization-path--required-categories)). This doc sets **how much evidence** each required category needs when the session is doc-only.

| Init path | Doc-only typical? | Notes |
| --- | --- | --- |
| **Small** | Often | README + roadmap + 1–2 rules + minimal checklist + handoff doc |
| **Medium** | Often | + ADRs, conventions, stack **docs** (not necessarily runnable stack) |
| **Large** | Sometimes for early phases | Agent/orch scaffold is still doc-only until code tasks start |

---

## Evidence tiers (`VAL-EVID-*`)

Use tiers in `Evidence:` bullets and handoff summaries. **Higher tiers subsume lower** (T3 implies T2 and T1 were done).

| Tier | ID | What it proves | Typical methods | When required |
| --- | --- | --- | --- | --- |
| **0 — Explicit N/A** | `VAL-EVID-00` | Category does not apply to this init | One-line reason + init path | Optional categories; **VAL-CAT-07** on doc-only init |
| **1 — Presence** | `VAL-EVID-01` | File or directory exists at committed path | `test -f`, glob, `git ls-files` | Every required scaffold path |
| **2 — Mechanical** | `VAL-EVID-02` | Links resolve; forbidden patterns absent | `rg`, link scan from [`handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md) § Suggested scans | **VAL-CAT-02**, **06**; rules index |
| **3 — Contract** | `VAL-EVID-03` | Content matches a named contract (outline, table, activation mode) | Read file + checklist row pass/fail | README outline, ADR index shape, rules index |
| **4 — Consistency** | `VAL-EVID-04` | Two artifacts agree (README ↔ backlog ↔ ADR ↔ rules) | Cross-walk tables in [`readme-sync.md`](../target-roadmap/readme-sync.md) | Before **VAL-CAT-06** final pass |
| **5 — Human** | `VAL-EVID-05` | User accepted residual risk or optional gaps | `PROJ-HANDOFF-003`; partial handoff note | Layer 3 handoff always |

**Do not use at doc-only init:**

| Forbidden at init | Use instead |
| --- | --- |
| **MG-01**–**MG-05** | Not applicable — merge-ready is **change** phase ([`gates-and-checks.md`](../orchestration/gates-and-checks.md)) |
| Lifecycle `gate_status` | No task folder required for small init |
| `validation-report.md` | Category bullets on `PROJ-*` + handoff checklist |
| Invented `pnpm run …` / test commands | **VAL-EVID-00** on **VAL-CAT-07** or defer `PROJ-STACK-*` |

---

## Category minimums (doc-only init)

For each **required** category (per init path matrix), meet the **minimum tier** below. Record on the relevant `PROJ-*` completion or on `PROJ-HANDOFF-001` rollup.

| Category | Doc-only minimum tiers | Evidence content (one line each) |
| --- | --- | --- |
| **VAL-CAT-01** Scaffold | T1 + T3 | List paths created vs [`init-paths.md` § A.3](../orchestration/init-paths.md#a3-proj--scope-matrix-by-init-path); note minimal `docs/validation-checklist.md` on **all** paths |
| **VAL-CAT-02** README & routing | T2 + T3 + T4 | Outline audit date; Documentation map links; [`handoff-checklist.md`](../target-readme/handoff-checklist.md) pass |
| **VAL-CAT-03** ADR alignment | T1 + T3 (when required) | `adrs/INDEX.md` / `GOVERNANCE.md`; Accepted vs open `PROJ-ADR-*`; or **VAL-EVID-00** with user-approved deferral |
| **VAL-CAT-04** Rules & agents | T1 + T3 | `.cursor/rules/index.md` activation; agent INDEX if large path |
| **VAL-CAT-05** Stack fidelity | T1 + T3 **or** **VAL-EVID-00** | If README claims stack but no manifests: open `PROJ-STACK-*`, no invented commands ([`commands.md` § Greenfield](../stack-scaffolding/commands.md#greenfield-and-doc-only-repos)) |
| **VAL-CAT-06** Independence | T2 + T3 + T4 | `handoff-self-containment.md` IND-* (+ ORCH-IND-* if orch exists); no Jarvis URLs in required reading |
| **VAL-CAT-07** Executable | **VAL-EVID-00 only** | Fixed phrase: `N/A — doc-only init; no runnable artifact produced` |
| **VAL-CAT-08** Orchestration | T1 + T3 **or** **VAL-EVID-00** | Large/medium with `_template/`: manifest shape + sanitized README; else N/A |

### VAL-CAT-07 (mandatory N/A line)

Every doc-only init handoff must include **exactly one** visible record:

```text
VAL-CAT-07: N/A — doc-only init; no runnable artifact produced (VAL-EVID-00)
```

Place it on:

- `PROJ-HANDOFF-001` or `PROJ-HANDOFF-002` `Evidence:` sub-bullet, **or**
- `docs/roadmap/README.md` handoff status paragraph (one line under **Handoff status:**)

Do **not** mark **TST-*** or **MG-05** rows in the target checklist as pass for init — leave them for **change** phase or note “init N/A” in checklist header comment only if the team wants a visible reminder.

---

## `Evidence:` bullet format (target backlog)

Extend [`conventions.md`](../target-roadmap/conventions.md#evidence) for initialization audits.

### Canonical pattern

```markdown
  - Evidence: VAL-CAT-02 PASS — VAL-EVID-02/03: README § Documentation links verified 2026-05-27; paths: README.md, docs/roadmap/README.md
```

| Segment | Required | Rules |
| --- | --- | --- |
| `VAL-CAT-0N` | Yes | Match category audited |
| `PASS` / `FAIL` / `N/A` | Yes | `N/A` only with **VAL-EVID-00** reason |
| `VAL-EVID-0N` | Yes | Lowest tier that satisfies minimum; use `/` when multiple (e.g. `02/03`) |
| Body | Yes | Date (`YYYY-MM-DD`); concrete paths or scan summary — no “looks good” |

### Examples by task type

**Scaffold task:**

```markdown
- [x] `PROJ-DOC-002`: Add minimal validation checklist. **required for handoff**
  - Evidence: VAL-CAT-01 PASS — VAL-EVID-01/03: `docs/validation-checklist.md` from template; MG/TST rows present, unchecked for init.
```

**Stack doc without manifests:**

```markdown
- [x] `PROJ-STACK-001`: Document commands from manifests. **required for handoff**
  - Evidence: VAL-CAT-05 N/A — VAL-EVID-00: doc-only init, no `package.json`; README § Development omitted; `PROJ-STACK-001` deferred until tooling added.
```

**Handoff gate rollup:**

```markdown
- [x] `PROJ-HANDOFF-001`: No Jarvis links in generated docs. **required for handoff**
  - Evidence: VAL-CAT-06 PASS — VAL-EVID-02: IND-01–IND-17 pass 2026-05-27; `rg -i jarvis` clean outside roadmap provenance.
  - Evidence: VAL-CAT-07 N/A — doc-only init; no runnable artifact produced (VAL-EVID-00).
```

### Optional init rollup (large inits only)

On **large** paths with many `PROJ-*` rows, agents may add a **single** table to `docs/roadmap/README.md` (not a new required file):

| Category | Result | Tiers | Date |
| --- | --- | --- | --- |
| VAL-CAT-01 | PASS | 01/03 | 2026-05-27 |
| … | … | … | … |

**Default:** omit table — per-task `Evidence:` is enough for small/medium.

---

## Agent workflow (doc-only init closeout)

1. Confirm init path → list required categories from [`categories.md`](./categories.md#initialization-path--required-categories).
2. Confirm session is **doc-only** per [definition](#documentation-only-initiation-definition) — if not, switch to `JR-VALIDATION-003` for runnable slices.
3. For each required category, run minimum tiers → record `PASS` / `N/A` on completing `PROJ-*` rows.
4. Run **VAL-CAT-06** last (T2–T4 + handoff checklist).
5. Write mandatory **VAL-CAT-07** N/A line.
6. Complete **VAL-EVID-05** via `PROJ-HANDOFF-003`.
7. Claim handoff only per [`handoff.md`](../target-roadmap/handoff.md) — do not cite **MG-*** or orchestrated merge-ready.

### Suggested commands (from target repo root)

Mechanical tier (**VAL-EVID-02**) — adjust scaffold name if not Jarvis:

```bash
# Forbidden required-reading links (example)
rg -i 'github\.com/[^/]+/jarvis|JR-[A-Z]+-[0-9]+' --glob '!docs/roadmap/**' README.md docs adrs .cursor .github

# Broken relative links in README Documentation section (manual follow-up if rg hits)
rg '\]\(\./|\]\(docs/' README.md
```

Presence tier (**VAL-EVID-01**) — example small path:

```bash
test -f README.md && test -f docs/roadmap/backlog.md && test -f docs/handoff-self-containment.md && test -f docs/validation-checklist.md
```

Do **not** fail doc-only init solely because `pnpm run check` cannot run — that is **VAL-CAT-07**, not **VAL-CAT-01**.

---

## Checklist and validation-report boundaries

| Artifact | Doc-only init |
| --- | --- |
| `docs/validation-checklist.md` | **Required** on all init paths (minimal rows) — copy and trim per [`../universal-validation/README.md`](../universal-validation/README.md); do not require every **MG-*** row to pass at init |
| `validation-report.md` | **Not** required — no Validator stage for linear init |
| PR template merge-ready section | **N/A** until first change PR |

Targets may add a checklist header note:

```markdown
<!-- Init note: MG-* and TST-* rows apply at change time (VAL-PHASE-CHANGE). Doc-only init records VAL-CAT-07 N/A per project validation guide. -->
```

---

## Human input (pause points)

Jarvis must **stop and ask** before:

| Situation | Why |
| --- | --- |
| User wants handoff while README § Development lists commands that were **never** verified | Becomes **VAL-CAT-05** / **07** failure or requires runnable evidence (`003`) |
| Mixed init (docs + generated code) without split evidence plan | Wrong tier application |
| Skipping minimal `validation-checklist.md` on small path | Conflicts with [`init-paths.md`](../orchestration/init-paths.md) human decision 2026-05-19 |
| Marking **MG-03** pass at init to “speed up” handoff | Conflates handoff with merge-ready |
| **VAL-CAT-03** skipped on medium/large without documented `PROJ-ADR-*` deferral | Architecture debt invisible |
| Replacing `VAL-EVID-*` tier IDs in Accepted ADRs or closed `PROJ-*` | Breaks audit trail |

**Defaults without asking** (notify user):

- Doc-only init → mandatory **VAL-CAT-07** N/A line
- No manifests → **VAL-CAT-05** N/A + open `PROJ-STACK-*` instead of inventing scripts
- Evidence on each **required for handoff** `PROJ-*` — rollup table optional on large path only

---

## Decisions recorded for `JR-VALIDATION-002`

| Topic | Decision |
| --- | --- |
| Canonical doc | This file — `doc-only-init-evidence.md` |
| Evidence tier IDs | **VAL-EVID-00** (N/A) through **VAL-EVID-05** (human) |
| **VAL-CAT-07** at doc-only init | Always **VAL-EVID-00** with fixed N/A phrase |
| **MG-*** at init | Never required for doc-only init |
| Minimal checklist | Required all init paths; row execution deferred to change phase |
| Separate `validation-init-summary.md` | **No** by default — `PROJ-*` `Evidence:` + optional README table on large |
| Existing repo with code | Init audit still doc-only for scaffold work; no full-repo test mandate |
| WFD | Tier discipline only — no WFD product or gap identifiers |
| Next task | `JR-VALIDATION-003` — runnable/config init evidence; `004` — self-containment tier extensions; `005` — Jarvis vs human completion claims |

## Related

| Doc | Role |
| --- | --- |
| [`README.md`](./README.md) | Validation pack index |
| [`categories.md`](./categories.md) | **VAL-CAT-*** catalog and init matrix |
| [`../orchestration/init-paths.md`](../orchestration/init-paths.md) | Init path vs run tier |
| [`../orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md) | Why **MG-*** ≠ init |
| [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | Three layers + claim wording |
| [`../stack-scaffolding/commands.md`](../stack-scaffolding/commands.md) | Greenfield / no manifests |
| [`../universal-validation/README.md`](../universal-validation/README.md) | Checklist copy |
