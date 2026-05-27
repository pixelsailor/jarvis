# Orchestration self-containment (`JR-ORCH-007`)

Canonical rules so **copied orchestration artifacts** in a target project work without the Jarvis repository, sibling workspace paths, or chat-only setup. Complements repo-wide handoff ([`../universal-handoff/README.md`](../universal-handoff/README.md) → target `docs/handoff-self-containment.md`) with **orchestration-specific** copy-time sanitization and verification rows (**ORCH-IND-*** ).

**Prerequisites:** [`task-folder-model.md`](./task-folder-model.md), [`artifact-ownership.md`](./artifact-ownership.md), [`../templates/orchestration/_template/`](../templates/orchestration/_template/)

## Design goals

| Goal | How this document helps |
| --- | --- |
| **Agent efficiency** | One copy checklist + one verification block — agents do not re-derive from WFD or Jarvis layout |
| **Fresh-session resume** | Task folders and agent contracts cite **target paths only** |
| **Clear ownership** | Copy-time = Jarvis init agent; verification = Orchestrator or init agent before `PROJ-ORCH-*` / `PROJ-HANDOFF-*` |
| **No doc sprawl** | Operational checks live in target `docs/handoff-self-containment.md` § Orchestration — not a second target checklist file |

## Scope — what must be self-contained

| Artifact | Target path (default) | Self-contained means |
| --- | --- | --- |
| Template pack | `.cursor/orchestrations/_template/` | No Jarvis URLs, `JR-*`, or `../../../orchestration/` links; placeholders replaced or documented in target guide |
| Per-run folder | `.cursor/orchestrations/{task-id}/` | Same; `task_id` matches folder slug; manifest has no scaffolding-repo fields |
| Orchestration guide | `docs/ORCHESTRATED_DEVELOPMENT.md` (name chosen by project) | Paraphrases platform contracts — **no** Jarvis links as required reading |
| Agent roster + contracts | `.cursor/agents/INDEX.md`, `.cursor/agents/*.md` | Binding references point at target guide, checklist, commands doc |
| Merge-ready rule | `.cursor/rules/workflow-gates.mdc` (when present) | Cites target checklist and orchestration paths only |
| Validation checklist appendix | `docs/validation-checklist.md` (when present) | Lifecycle / handoff narrative is target-owned — no “read Jarvis `init-paths`” |

**Out of scope:** Jarvis platform docs under `jarvis/docs/orchestration/` — those may link to Jarvis paths for maintainers only.

## Two phases

```text
Copy (init)                    Verify (before handoff / each bulk import)
─────────────────────────────  ───────────────────────────────────────────
Sanitize _template/ pack   →   ORCH-IND-01 … ORCH-IND-10 in
Replace placeholders           docs/handoff-self-containment.md
Author orchestration guide     + IND-01 … IND-17 (repo-wide)
Wire agents + rules
```

Re-run **verification** when adding orchestration after handoff, importing a new template pack version, or opening a new `PROJ-ORCH-*` tranche.

## Copy-time sanitization (Jarvis init agent)

Perform **in the target repository** immediately after copying [`../templates/orchestration/_template/`](../templates/orchestration/_template/) → `.cursor/orchestrations/_template/`.

### Step 1 — Copy files

| Source (Jarvis) | Target |
| --- | --- |
| `_template/*` (manifest example + seven markdown templates + `README.example.md`) | `.cursor/orchestrations/_template/` |
| Rename | `task-manifest.example.json` stays as example until bootstrap; per-run copy becomes `task-manifest.json` |
| README | Copy `README.example.md` → `_template/README.md` **after** sanitization (below) — do not leave Jarvis-only `README.example.md` as the only reference |

### Step 2 — Strip scaffolding-only content

Remove or rewrite any line that:

- Links to `jarvis`, `docs/orchestration/` **outside** the target repo, or `../templates/`
- Names **`JR-*`** platform task IDs as required reading
- Says **“copy from Jarvis”** without naming a **target-local** file that already exists
- References **WFD** product IDs, gap IDs, or WFD-only paths
- Uses **unreplaced** `REPLACE_WITH_*` except in the manifest **example** filename before first run

**Template callouts** (lines starting with `> **Template**` or `**Jarvis:**`) are for copy sessions only — **delete** them in the target `_template/README.md` and in target agent files before marking `PROJ-ORCH-001` complete.

### Step 3 — Replace placeholders

| Placeholder | Set to (target-local) |
| --- | --- |
| `REPLACE_WITH_PROJECT_SLUG` | Chosen once at init (e.g. `acme` → task IDs `acme-001`) |
| `REPLACE_WITH_PROJECT_SLUG-000` / `REPLACE_WITH_TASK_ID` | Example slug + sequence or real `task_id` at bootstrap |
| `REPLACE_WITH_ORCHESTRATION_GUIDE_PATH` | e.g. `docs/ORCHESTRATED_DEVELOPMENT.md` |
| `REPLACE_WITH_VALIDATION_CHECKLIST_PATH` | e.g. `docs/validation-checklist.md` |
| `REPLACE_WITH_TEST_MATRIX_PATH` | Path documented in testing-strategy or orchestration guide |
| `REPLACE_WITH_COMMANDS_DOC` | e.g. `docs/stack/commands.md` |
| `REPLACE_WITH_VERIFIED_TEST_COMMAND` | Literal command from verified manifests — **never** invent |

Agent templates: same placeholders — see [`../universal-agents/README.md`](../universal-agents/README.md) § Copy and adapt.

### Step 4 — Author target orchestration guide

Jarvis does **not** copy WFD's orchestration guide verbatim. The target guide must include (paraphrased, target-owned):

- Task-folder root and `{project-slug}-NNN` convention
- Lifecycle gates 0–6 vs merge-ready **MG-*** vs **PROJ-HANDOFF-***
- Small / medium / large **run** tiers and paths A/B/C (from [`init-paths.md`](./init-paths.md) — paraphrase only)
- Loop and rework behavior (from [`loops-and-rework.md`](./loops-and-rework.md) — paraphrase only)
- Artifact ownership summary (from [`artifact-ownership.md`](./artifact-ownership.md) — paraphrase only)
- Pointer to `.cursor/orchestrations/_template/README.md` for per-run bootstrap

### Step 5 — Wire agents and rules

| File | Must cite |
| --- | --- |
| `.cursor/agents/INDEX.md` | Target orchestration guide, validation checklist, commands doc |
| Each `.cursor/agents/*.md` | Same; artifact paths under `.cursor/orchestrations/{task-id}/` |
| `.cursor/rules/workflow-gates.mdc` (if copied) | Target checklist + orchestration guide — not Jarvis |

## Verification — ORCH-IND checks

Target agents record pass/fail in `docs/handoff-self-containment.md` (template § Orchestration artifacts). **Fail** on any required row blocks `PROJ-ORCH-001` completion and **`PROJ-HANDOFF-001` / `002`** until fixed or user approves partial handoff.

| ID | Check |
| --- | --- |
| **ORCH-IND-01** | No `http`/`https` links to Jarvis (or other scaffold repo) in `.cursor/orchestrations/`, `.cursor/agents/`, orchestration guide, or `workflow-gates.mdc` **as required reading** |
| **ORCH-IND-02** | No `JR-*` IDs in `.cursor/orchestrations/` or orchestration guide (historical `docs/roadmap/` prose only) |
| **ORCH-IND-03** | No relative paths escaping the repo to reach Jarvis (`../jarvis/`, `../../Projects/jarvis/`, `../../../orchestration/`) |
| **ORCH-IND-04** | `_template/README.md` (if present) cites only target paths — no Jarvis maintainer links |
| **ORCH-IND-05** | Every `REPLACE_WITH_*` in **required** orchestration paths is replaced (manifest example may keep slug placeholder until first run — document in backlog) |
| **ORCH-IND-06** | Target orchestration guide exists and is linked from `.cursor/agents/INDEX.md` |
| **ORCH-IND-07** | `task-manifest.json` in active runs contains no scaffolding-repo URLs or foreign schema doc links |
| **ORCH-IND-08** | Per-run markdown (`plan.md`, `validation-report.md`, etc.) references target ADRs, rules, and `docs/` — not Jarvis |
| **ORCH-IND-09** | Validation checklist orchestration appendix does not require opening Jarvis docs to interpret lifecycle vs **MG-*** vs handoff |
| **ORCH-IND-10** | After bootstrap, `{task-id}/` folder name **equals** manifest `task_id` exactly |

### Suggested scans (target repo root)

```bash
rg -i 'jarvis|JR-[A-Z]+-[0-9]+|\.\./.*orchestration' .cursor/orchestrations .cursor/agents 2>/dev/null || true
rg 'REPLACE_WITH_' .cursor/orchestrations .cursor/agents docs/ORCHESTRATED_DEVELOPMENT.md 2>/dev/null || true
```

Review hits: deferred `PROJ-*` tasks may document intentional placeholders; **active** orchestration paths may not.

## Backlog evidence

When completing orchestration scaffold tasks:

```markdown
- [x] `PROJ-ORCH-001`: Seed `.cursor/orchestrations/_template/` … **required for handoff** (large path)
  - Evidence: ORCH-IND-01–ORCH-IND-10 pass 2026-05-19; `docs/ORCHESTRATED_DEVELOPMENT.md`; `_template/README.md` sanitized.

- [x] `PROJ-HANDOFF-002`: Confirm rules and agents cite only target-project paths.
  - Evidence: IND-07–IND-09 + ORCH-IND-01–ORCH-IND-06 pass; sampled `orchestrator.md`, `workflow-gates.mdc`.
```

Optional `PROJ-ORCH-002`: Re-run ORCH-IND after template pack upgrade — only when the team tracks pack versions separately.

## Relationship to other docs

| Doc | Relationship |
| --- | --- |
| [`../universal-handoff/README.md`](../universal-handoff/README.md) | Repo-wide **IND-*** ; orchestration rows are additive |
| [`gates-and-checks.md`](./gates-and-checks.md) | Handoff vocabulary — target guide paraphrase, no Jarvis links |
| [`artifact-ownership.md`](./artifact-ownership.md) | § Target adoption — copy step defers sanitization detail here |
| [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | `PROJ-ORCH-*` optional on small path; required on large |
| **`JR-VALIDATION-002`** | Doc-only init evidence tiers — [`../validation/doc-only-init-evidence.md`](../validation/doc-only-init-evidence.md) |
| **`JR-VALIDATION-004`** | May add evidence tiers later — ORCH-IND remains the operational orchestration slice |

## Human input (pause points)

Jarvis or the init agent must **stop and ask** before:

- Leaving **Jarvis URLs** in target orchestration or agent files because the team uses a **monorepo** with Jarvis as a sibling (document an approved exception in target `docs/roadmap/` and narrow required-reading scope).
- **Skipping** the target orchestration guide on large init (agents will re-derive gates from chat).
- Shipping `_template/` with **unsanitized** `README.example.md` (Jarvis links) as the only README in `_template/`.
- Pointing agent **minimum read sets** at Jarvis platform docs instead of target INDEX § Minimum read sets + checklist — use [`../universal-agents/minimum-read-sets.md`](../universal-agents/minimum-read-sets.md) when scaffolding; targets must not list Jarvis paths as mandatory reads (**IND-08**).
- **Committing** filled `.cursor/orchestrations/{task-id}/` to git when the team default is local-only ([`task-folder-model.md` § Git policy](./task-folder-model.md#git-policy)).

Routine copy, placeholder replacement, ORCH-IND verification, and backlog evidence sub-bullets do **not** require extra approval.

## Decisions recorded for `JR-ORCH-007`

| Topic | Decision |
| --- | --- |
| Canonical platform doc | This file — Jarvis maintainers and init agents only |
| Target operational checks | **ORCH-IND-01–10** in `docs/handoff-self-containment.md` — no separate `docs/orchestration-self-containment.md` in targets |
| `_template/README` | Sanitized copy as `_template/README.md`; Jarvis keeps `README.example.md` with maintainer note pointing here |
| Placeholder policy | No unreplaced `REPLACE_WITH_*` in required paths at `PROJ-ORCH-001` complete |
| WFD / Jarvis IDs | Never in target `.cursor/orchestrations/` or orchestration guide |
| Overlap with `JR-UNIVERSAL-007` | Handoff doc gains orchestration section; whole-repo **IND-*** still required |
| Re-verify triggers | New template import, post-handoff orchestration enablement, bulk agent contract refresh |

## Related

| Doc | Role |
| --- | --- |
| [`README.md`](./README.md) | Orchestration pack index |
| [`../templates/orchestration/_template/README.example.md`](../templates/orchestration/_template/README.example.md) | Target-safe template README (copy after sanitization rules) |
| [`../templates/universal-handoff/handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md) | ORCH-IND rows for targets |
