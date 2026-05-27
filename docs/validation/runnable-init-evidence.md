# Runnable initialization — evidence expectations (`JR-VALIDATION-003`)

Canonical **evidence tiers** and **audit rules** when Jarvis closes **initialization** (`VAL-PHASE-INIT`) after producing or changing **runnable** artifacts: application source, tests, lockfiles/manifests, CI/workflows, service workers, or deployable config. Use with category scoping in [`categories.md`](./categories.md) (`JR-VALIDATION-001`), command extraction in [`../stack-scaffolding/commands.md`](../stack-scaffolding/commands.md) (`JR-STACK-003`), and init path matrices in [`../orchestration/init-paths.md`](../orchestration/init-paths.md) (`JR-ORCH-005`).

**Doc-only init evidence:** [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) (`JR-VALIDATION-002`) — when init produces no runnable delta.  
**Self-containment operations:** [`../templates/universal-handoff/handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md) + **VAL-CAT-06** — `JR-VALIDATION-004` (planned).  
**Jarvis completion claims:** `JR-VALIDATION-005` (planned).

## Design goals

| Goal | How this document helps |
| --- | --- |
| **Agent efficiency** | One **default quality chain** + extended **VAL-EVID-*** tiers — no re-negotiating bar per session |
| **Readable audits** | `Evidence:` bullets cite `VAL-CAT-07` + `VAL-EVID-06/07` + exit codes — not chat claims |
| **Resume-friendly** | Cold sessions re-run the same commands from README / `docs/stack/commands.md` |
| **Vocabulary safety** | Init runnable evidence ≠ orchestrated **MG-03** / **MG-04** / full **TST-*** closure |

---

## When this doc applies

### Runnable initialization (definition)

An init session (or init **phase** of a larger path) is **runnable** when **any** of the following hold for work **in scope for this init** (Jarvis-generated or user-directed init tasks — not mandatory full-repo regression unless the user requests it):

| Criterion | Examples |
| --- | --- |
| **Executable source delta** | New/changed product `.ts`, `.js`, `.py`, `.go`, `.rs`, `.java`, `.svelte`, etc.; new/changed test files meant to run in CI |
| **Manifest / lockfile delta** | `package.json` scripts added or changed; new lockfile; `pyproject.toml` / `Cargo.toml` / `go.mod` tooling changes |
| **Runnable config delta** | New/changed CI workflow that runs build/test; service worker; bundler config that affects `dev` / `build` / `check` |
| **README claims commands** | README § Development lists install/dev/check/lint/test invocations Jarvis expects agents to run at handoff |

**Still runnable** when the **target repo already contained** application code — evidence covers **init-scoped** commands and paths, not re-proving unrelated legacy modules.

**Not runnable** (use [`doc-only-init-evidence.md`](./doc-only-init-evidence.md)):

- Init is documentation + agent config only (see doc-only [definition](./doc-only-init-evidence.md#documentation-only-initiation-definition))

**Mixed init** (mostly docs + one runnable slice):

- Default: session is **not** doc-only.
- **Split evidence** is allowed only with **explicit user approval**: doc categories per `002`; **VAL-CAT-05** / **VAL-CAT-07** per this doc for the runnable slice only.

### Relationship to init path

Init path sets **which categories are required** ([`categories.md` § Initialization path → required categories](./categories.md#initialization-path--required-categories)). This doc sets **how much evidence** **VAL-CAT-05** and **VAL-CAT-07** need when the session is runnable.

| Init path | Runnable typical? | **VAL-CAT-07** expectation |
| --- | --- | --- |
| **Small** | Sometimes | Default chain when manifests exist; may defer execution with user approval |
| **Medium** | Often | Default chain required when `PROJ-STACK-001` closes or README § Development exists |
| **Large** | Often | Same + record on `PROJ-STACK-*` and handoff rollup |

---

## Evidence tiers (`VAL-EVID-*`)

Tiers **00–05** are defined in [`doc-only-init-evidence.md`](./doc-only-init-evidence.md). **Runnable init** adds **06–07** on top of doc tiers where categories require them. **Higher runnable tiers subsume lower** (07 implies 06 and manifest inspection).

| Tier | ID | What it proves | Typical methods | When required (runnable init) |
| --- | --- | --- | --- | --- |
| **0 — Explicit N/A** | `VAL-EVID-00` | Category or command does not apply | One-line reason | Optional categories; no scripts in manifests |
| **1 — Presence** | `VAL-EVID-01` | File exists at committed path | `test -f`, `git ls-files` | Changed manifests, CI files, generated paths |
| **2 — Mechanical** | `VAL-EVID-02` | Links resolve; forbidden patterns absent | `rg`, link scan | **VAL-CAT-02**, **06** (unchanged) |
| **3 — Contract** | `VAL-EVID-03` | Content matches named contract | Outline, commands table shape | **VAL-CAT-05** docs vs template |
| **4 — Consistency** | `VAL-EVID-04` | README ↔ `commands.md` ↔ checklist placeholders agree | Cross-walk | Before **VAL-CAT-06**; before claiming **07** pass |
| **5 — Human** | `VAL-EVID-05` | User accepted residual risk | `PROJ-HANDOFF-003` | Layer 3 handoff; execution deferral |
| **6 — Manifest trace** | `VAL-EVID-06` | Each documented command maps to manifest/CI/Makefile | Extraction pass in [`commands.md`](../stack-scaffolding/commands.md) | **VAL-CAT-05** pass; prerequisite for **07** |
| **7 — Executed** | `VAL-EVID-07` | Command ran in target repo; exit code recorded | Run default quality chain once | **VAL-CAT-07** pass when claiming runnable handoff |

**Do not use at runnable init (unless user runs a full orchestrated init task):**

| Forbidden at init | Use instead |
| --- | --- |
| **MG-03** `validation-report.md` required | `PROJ-*` `Evidence:` + optional README rollup table (large) |
| **MG-04** `test-report.md` required | **VAL-EVID-07** on default **test** script; full **TST-*** at **VAL-PHASE-CHANGE** |
| Full **TST-01**–**TST-05** closure | Init records chain execution; checklist rows may note `init: VAL-EVID-07` in header comment |
| Invented commands | Fail **VAL-CAT-05** / **07** or defer with **VAL-EVID-05** + open `PROJ-STACK-*` |

---

## Default quality chain

The **default quality chain** is the **minimal set of verified invocations** agents run once before runnable init handoff. Derive it only from target manifests ([`commands.md` § Classify scripts](../stack-scaffolding/commands.md#3-classify-scripts-do-not-invent-keys)).

### Build the chain

| Step | Include when | Skip when |
| --- | --- | --- |
| **install** | Dependencies not yet installed in audit environment | User confirms deps pre-installed; record `install: skipped (pre-installed)` |
| **check** | Script key exists (`check`, `typecheck`, `verify`, …) | No key — do not invent `tsc` |
| **lint** | Separate `lint` script exists | Lint folded into `check` only — do not run twice |
| **test** | `test` or agreed default test script exists | No test runner — **VAL-EVID-00** on test step with reason |
| **build** | Init added/changed build tooling or README lists build at handoff | Doc-only stack profile; user defers build |

**Order:** `install` → `check` → `lint` → `test` → `build` (omit missing steps).

**Monorepo:** Run from **primary package path** in `docs/stack/stack-profile.md`; prefix evidence with path.

**CI-only commands:** Document in `docs/stack/commands.md` with **CI** vs **local** labels. Default chain uses **local** README invocations unless the user selects CI as handoff default ([`commands.md` pause points](../stack-scaffolding/commands.md#human-input-pause-points)).

### Execution record (`VAL-EVID-07`)

For each command in the chain, record:

| Field | Required | Example |
| --- | --- | --- |
| **invocation** | Yes | `pnpm run check` |
| **cwd** | When not repo root | `apps/web` |
| **exit_code** | Yes | `0` or non-zero |
| **date** | Yes | `YYYY-MM-DD` |
| **summary** | Yes | One line: `passed`, `failed: 12 type errors`, `not run: install timeout` |
| **log** | No | Do not paste full stdout in backlog — path to CI run or “local terminal” is enough |

**Pass rule for VAL-CAT-07:** All **required** chain steps for this init have `exit_code: 0`, **or** user accepted deferral via **VAL-EVID-05** with open `PROJ-STACK-*` / handoff note (see [pause points](#human-input-pause-points)).

**Fail rule:** README or `commands.md` claims a command **passed** but evidence shows non-zero exit or `not run` without deferral → **VAL-CAT-07 FAIL**; do not claim init handoff complete.

---

## Category minimums (runnable init)

For each **required** category (per init path matrix), meet the **minimum tier** below. Record on completing `PROJ-*` rows or `PROJ-HANDOFF-*` rollup.

| Category | Runnable minimum tiers | Evidence content (one line each) |
| --- | --- | --- |
| **VAL-CAT-01** Scaffold | T1 + T3 | Paths created; note if `validation-checklist.md` placeholders filled from chain |
| **VAL-CAT-02** README & routing | T2 + T3 + T4 | § Development matches manifests when present |
| **VAL-CAT-03** ADR alignment | T1 + T3 (when required) | Same as doc-only |
| **VAL-CAT-04** Rules & agents | T1 + T3 | Same as doc-only |
| **VAL-CAT-05** Stack fidelity | T1 + T3 + **T6** | `commands.md` / README § Development; manifest paths + `last_verified` date |
| **VAL-CAT-06** Independence | T2 + T3 + T4 | Run last; handoff checklist |
| **VAL-CAT-07** Executable | **T6 + T7** | Default quality chain with exit codes; or **FAIL** / deferral per policy |
| **VAL-CAT-08** Orchestration | T1 + T3 or **VAL-EVID-00** | Unchanged from doc-only |

### VAL-CAT-07 (mandatory pass or deferral line)

Every runnable init handoff must include **exactly one** of:

**Pass (chain executed):**

```text
VAL-CAT-07: PASS — VAL-EVID-06/07: default chain 2026-05-27 — install:0, check:0, lint:0, test:0 (pnpm run check, pnpm run lint, pnpm run test)
```

**Deferred (user-approved):**

```text
VAL-CAT-07: DEFERRED — VAL-EVID-05/06: manifest trace only; execution deferred to PROJ-STACK-001; user sign-off PROJ-HANDOFF-003 2026-05-27
```

**N/A (no test/check scripts in repo):**

```text
VAL-CAT-07: N/A — VAL-EVID-00: manifests have no check/lint/test scripts; README § Development omits quality chain
```

Place on `PROJ-HANDOFF-001` / `PROJ-HANDOFF-002` `Evidence:` sub-bullet or `docs/roadmap/README.md` **Handoff status:** paragraph.

Do **not** mark all **MG-05** / **TST-*** checklist rows as pass at init — use header comment per [`doc-only-init-evidence.md` § Checklist boundaries](./doc-only-init-evidence.md#checklist-and-validation-report-boundaries) and add runnable note:

```markdown
<!-- Init note: MG-* and TST-* rows apply at change time (VAL-PHASE-CHANGE). Runnable init records VAL-CAT-07 per project validation guide (VAL-EVID-06/07). -->
```

---

## Runnable artifact slices

Use the slice that matches init work — evidence scope stays **init-scoped** unless the user expands it.

| Slice | **VAL-CAT-05** | **VAL-CAT-07** | Notes |
| --- | --- | --- | --- |
| **A — Manifests only** | T6 required | T7 on **install** only if claiming “deps installable”; else defer | New `package.json` without app source |
| **B — Config / CI only** | T6; CI job names trace to workflow file | T7 optional: run workflow locally (`act`) or record `execution: CI not run locally` + user deferral | Do not claim workflow green without run or deferral |
| **C — Source + manifests** | T6 + T4 | Full default chain | Typical medium/large init |
| **D — Scripts added to existing repo** | T6 for new keys only | T7 on new/changed scripts + affected chain | Pre-existing failures: document `pre-existing: check fails on main` and do not claim **07** pass for untouched failures without user acceptance |

---

## `Evidence:` bullet format (target backlog)

Same segments as [`doc-only-init-evidence.md`](./doc-only-init-evidence.md#evidence-bullet-format-target-backlog), plus **execution** detail for **VAL-CAT-05** / **07**.

### Canonical patterns

**Stack + execution (`PROJ-STACK-001`):**

```markdown
- [x] `PROJ-STACK-001`: Document package manager and validation commands from actual project files. **required for handoff**
  - Evidence: VAL-CAT-05 PASS — VAL-EVID-06: `package.json` (.) inspected 2026-05-27; `docs/stack/commands.md`; README § Development aligned.
  - Evidence: VAL-CAT-07 PASS — VAL-EVID-07: `pnpm run check` exit 0, `pnpm run test` exit 0 (2026-05-27).
```

**Generated app shell (`PROJ-*` code task):**

```markdown
- [x] `PROJ-CODE-001`: Add minimal app entry and test script. **required for handoff**
  - Evidence: VAL-CAT-07 PASS — VAL-EVID-07: `pnpm run build` exit 0, `pnpm run test` exit 0; paths: `src/main.ts`, `package.json`.
```

**Manifest trace without execution (deferred):**

```markdown
- [ ] `PROJ-STACK-001`: Document package manager and validation commands. **required for handoff**
  - Evidence: VAL-CAT-05 PASS — VAL-EVID-06: manifests inspected 2026-05-27; README § Development drafted.
  - Evidence: VAL-CAT-07 DEFERRED — VAL-EVID-05: user deferred execution until CI available; see PROJ-HANDOFF-003.
```

| Segment | Runnable-specific rules |
| --- | --- |
| `DEFERRED` | Only with **VAL-EVID-05** + open `PROJ-STACK-*` or handoff sign-off |
| Body | Include `exit 0` / `exit 1` / `not run`; never “tests pass” without invocation |

### Checklist placeholder sync

When **VAL-EVID-07** passes, update `docs/validation-checklist.md`:

- `REPLACE_WITH_VERIFY_COMMANDS` ← comma-separated invocations from the **default quality chain** (same strings as README § Development minimal chain).

---

## Agent workflow (runnable init closeout)

1. Confirm init path → required categories from [`categories.md`](./categories.md#initialization-path--required-categories).
2. Confirm session is **runnable** per [definition](#runnable-initialization-definition) — if doc-only, switch to [`doc-only-init-evidence.md`](./doc-only-init-evidence.md).
3. Run [`commands.md` extraction pass](../stack-scaffolding/commands.md#extraction-pass) → **VAL-EVID-06** on **VAL-CAT-05**.
4. Build [default quality chain](#default-quality-chain) → run in target repo → **VAL-EVID-07** on **VAL-CAT-07** (or document deferral).
5. Complete other required categories (doc tiers 01–04 as applicable).
6. Run **VAL-CAT-06** last (T2–T4 + handoff checklist).
7. Write mandatory **VAL-CAT-07** pass / deferred / N/A line.
8. Complete **VAL-EVID-05** via `PROJ-HANDOFF-003` when deferring execution or residual risk exists.
9. Claim handoff only per [`handoff.md`](../target-roadmap/handoff.md) — do not cite orchestrated **MG-03** / **MG-04** unless init used a task folder with those stages.

### Suggested commands (from target repo root)

Adjust package manager and paths per `docs/stack/stack-profile.md`.

```bash
# Manifest trace (VAL-EVID-06) — example Node
test -f package.json && node -e "const p=require('./package.json'); console.log(Object.keys(p.scripts||{}).sort().join(','))"

# Default chain (VAL-EVID-07) — example; run only verified invocations
pnpm install && pnpm run check && pnpm run lint && pnpm run test
```

Record exit codes from `$?` after each step (or single compound with documented failure point).

---

## Checklist and validation-report boundaries

| Artifact | Runnable init |
| --- | --- |
| `docs/validation-checklist.md` | Required on all init paths; fill `REPLACE_WITH_VERIFY_COMMANDS` when **VAL-EVID-07** passes |
| **MG-05** row | May note `init: satisfied via VAL-CAT-07 (VAL-EVID-07)` in checklist header — do not require full PR merge-ready |
| `validation-report.md` | **Not** required for linear init |
| `test-report.md` | **Not** required — use **VAL-EVID-07** summary on `PROJ-*` |

---

## Human input (pause points)

Jarvis must **stop and ask** before:

| Situation | Why |
| --- | --- |
| README § Development lists commands **absent** from manifests | **VAL-CAT-05** failure or README fix |
| User wants **VAL-CAT-07 PASS** without running any command | Requires **VAL-EVID-05** deferral + explicit residual risk |
| Mixed init without **split evidence** plan | Wrong tier doc (002 vs 003) |
| Default chain passes but **pre-existing** repo failures unrelated to init scope | Whether **07** pass is allowed with documented baseline |
| CI workflow added but user forbids local/`act` run | **Slice B** — defer or accept CI-only verification policy |
| Replacing `VAL-EVID-*` tier IDs in Accepted ADRs or closed `PROJ-*` | Breaks audit trail |
| Marking **MG-03** / **TST-*** all pass at init to “speed up” handoff | Conflates init with **VAL-PHASE-CHANGE** |

**Defaults without asking** (notify user):

- Runnable init → **VAL-CAT-07** line required (pass, deferred, or N/A with reason)
- Commands documented only from manifests — no invented scripts
- **VAL-EVID-07** records exit codes, not full logs
- Init does not require `validation-report.md` / `test-report.md`
- Pre-existing failures outside init scope → **07** pass only for init-touched commands unless user expands scope

---

## Decisions recorded for `JR-VALIDATION-003`

| Topic | Decision |
| --- | --- |
| Canonical doc | This file — `runnable-init-evidence.md` |
| Runnable evidence tiers | **VAL-EVID-06** (manifest trace), **VAL-EVID-07** (executed); **00–05** shared with `002` |
| **VAL-CAT-07** at runnable init | **T6 + T7** pass, or **DEFERRED** with **T5**, or **N/A** with **00** when no scripts |
| Default quality chain | install (when needed) → check → lint → test → build (omit missing; no invented steps) |
| **MG-*** / **TST-*** at init | Not fully closed; **VAL-EVID-07** satisfies executable intent for handoff |
| `validation-report.md` / `test-report.md` | Not required at init |
| Evidence scope | Init-scoped artifacts and commands unless user expands |
| Mixed init | Not doc-only by default; split evidence only with user approval |
| WFD | Command discipline only — extract from **target** manifests, not WFD defaults |
| Next task | `JR-VALIDATION-004` — self-containment tier extensions; `005` — Jarvis vs human completion claims |

## Related

| Doc | Role |
| --- | --- |
| [`README.md`](./README.md) | Validation pack index |
| [`categories.md`](./categories.md) | **VAL-CAT-*** catalog |
| [`doc-only-init-evidence.md`](./doc-only-init-evidence.md) | Doc-only counterpart |
| [`../stack-scaffolding/commands.md`](../stack-scaffolding/commands.md) | Extraction and README sync |
| [`../orchestration/gates-and-checks.md`](../orchestration/gates-and-checks.md) | Why **MG-*** ≠ init |
| [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | Handoff layers |
| [`../universal-validation/README.md`](../universal-validation/README.md) | Checklist copy |
