# Universal agent roster and role contracts

Jarvis guidance for **target-project** multi-agent **rosters** and **role contracts**: an INDEX routes Orchestrator, Planner, Builder, Tester, and Validator work; per-role `.md` files define boundaries, artifact ownership, and gate responsibilities without embedding stack commands or product domains.

**Platform tasks:** `JR-AGENT-001` (roster INDEX), `JR-AGENT-002` (role contracts)  
**Copy source:** [`../templates/universal-agents/`](../templates/universal-agents/)  
**Target layout (default):**

| Path | Role |
| --- | --- |
| `.cursor/agents/INDEX.md` | Role roster, pipeline diagram, artifact table, gate summary, rule-binding placeholders |
| `.cursor/agents/orchestrator.md` | Orchestrator contract |
| `.cursor/agents/planner.md` | Planner contract |
| `.cursor/agents/builder.md` | Builder contract |
| `.cursor/agents/tester.md` | Tester contract |
| `.cursor/agents/validator.md` | Validator contract |

**Alternate path:** `agents/INDEX.md` and `agents/*.md` at repo root when the team keeps agents outside `.cursor/` — document the choice in the target orchestration guide and point `.cursor/rules/index.md` § Agent contracts at the canonical path.

**Jarvis repo:** Does **not** ship `.cursor/agents/` for Jarvis self-use; templates only.

## When to scaffold

| Initialization path | Agent roster (`INDEX.md`) | Role contracts (`.cursor/agents/*.md`) |
| --- | --- | --- |
| **Small** | Skip — no task-folder orchestration by default | Skip |
| **Medium** | **Optional** — roster-only INDEX when the team will run occasional orchestrated work | **Optional** unless copying `.cursor/orchestrations/_template/` or any active `PROJ-ORCH-*`; otherwise defer contracts until large init or orchestration is enabled |
| **Large** | **Required** — same session as `.cursor/orchestrations/_template/` | **Required** — copy five contracts in the same init wave as INDEX |

See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) (scaffolding step 6).

## Copy and adapt (agents)

### Roster (`INDEX.md`)

1. If the target already has an agent index or contracts, **audit** — merge roster tables; do not overwrite team contracts without user approval.
2. Create `.cursor/agents/` if missing (or use agreed alternate root).
3. Copy [`INDEX.example.md`](../templates/universal-agents/INDEX.example.md) → `.cursor/agents/INDEX.md`.
4. Replace placeholders at the top:
   - `REPLACE_WITH_PROJECT_NAME`
   - `REPLACE_WITH_ORCHESTRATION_GUIDE_PATH`
   - `REPLACE_WITH_VALIDATION_CHECKLIST_PATH`
   - `REPLACE_WITH_TEST_MATRIX_PATH`
5. Point **Binding references** and **Rule bindings** at the target `.cursor/rules/index.md`.
6. Align **Test runners** with [`../stack-scaffolding/testing.md`](../stack-scaffolding/testing.md) and `docs/stack/testing-strategy.md` — **no invented commands**.
7. Register `PROJ-AGENT-*` rows in `docs/roadmap/backlog.md` with evidence paths.
8. Link roster from root README § Documentation on **large** path; optional on **medium** when INDEX was copied early.

### Role contracts (five files)

1. Copy each template from [`../templates/universal-agents/`](../templates/universal-agents/):

   | Template | Target file |
   | --- | --- |
   | `orchestrator.example.md` | `.cursor/agents/orchestrator.md` |
   | `planner.example.md` | `.cursor/agents/planner.md` |
   | `builder.example.md` | `.cursor/agents/builder.md` |
   | `tester.example.md` | `.cursor/agents/tester.md` |
   | `validator.example.md` | `.cursor/agents/validator.md` |

2. Replace placeholders in every contract (and in INDEX if not done yet):
   - `REPLACE_WITH_ORCHESTRATION_GUIDE_PATH` — e.g. `docs/ORCHESTRATED_DEVELOPMENT.md`
   - `REPLACE_WITH_VALIDATION_CHECKLIST_PATH` — e.g. `docs/validation-checklist.md`
   - `REPLACE_WITH_TEST_MATRIX_PATH` — e.g. `docs/test-matrix.md` or path in testing-strategy doc
   - `REPLACE_WITH_COMMANDS_DOC` — e.g. `docs/stack/commands.md` (verified commands; no invented scripts)

3. Fill **Topic rules** tables from `.cursor/rules/index.md` (remove placeholder rows).
4. Cross-check INDEX **Binding references** lists all five contract paths.
5. Do not embed Jarvis `JR-*` IDs or Jarvis repository URLs in target artifacts.

**Handoff rule:** After copy, the target project must operate without reading Jarvis docs.

## Relationship to other artifacts

| Artifact | Relationship |
| --- | --- |
| [`../orchestration/README.md`](../orchestration/README.md) | Task folders, manifest schema, `pipeline` array semantics |
| [`../orchestration/task-folder-model.md`](../orchestration/task-folder-model.md) | Artifact filenames, lifecycle vocabulary, risk tiers |
| [`../orchestration/task-manifest.md`](../orchestration/task-manifest.md) | `current_agent`, `gate_status`, `risk_tier` enums |
| [`../templates/orchestration/`](../templates/orchestration/) | `_template/` for per-run folders |
| [`../universal-rules/README.md`](../universal-rules/README.md) | Rules index; topic rules referenced from roster § Rule bindings |
| [`../universal-validation/README.md`](../universal-validation/README.md) | Checklist path; Validator merge-ready **MG-*** rows |
| [`../universal-handoff/README.md`](../universal-handoff/README.md) | Independence audit after agents + orchestration land |

**Division of labor (platform backlog):**

| Task | Delivers |
| --- | --- |
| **`JR-AGENT-001`** | Roster / INDEX template |
| **`JR-AGENT-002`** | Five per-role contract templates |
| **`JR-AGENT-003`** | Minimum read set tables per role (efficiency) — **not** in contract templates |
| **`JR-AGENT-004`** | Handoff prompt / Next Agent Directive blocks for orchestration guide — **not** full duplicate in contracts |
| **`JR-ORCH-003`–`007`** | Artifact ownership detail, gates vs merge-ready, init paths, loops, self-containment |

Keep **small-run path matrices** thin in INDEX and contracts; defer nuance to **`JR-ORCH-005`** and the target orchestration guide.

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- Choosing **`agents/INDEX.md`** instead of **`.cursor/agents/INDEX.md`** when the repo already has `.cursor/rules/` and `.cursor/orchestrations/` (split roots confuse agents).
- Shipping **full role contracts** on a **medium** init when the user expected roster-only until large init.
- **Skipping** Planner or Validator on architecture-touching, security-sensitive, or ADR-governed work without explicit approval and manifest rationale.
- Replacing an existing agent roster or contracts the team already adopted.
- Embedding **stack test commands** in INDEX or contracts before `docs/stack/commands.md` or README § Development is verified.

Routine copy of INDEX and contracts, placeholder replacement, and backlog evidence sub-bullets do **not** require extra approval.

## Decisions recorded for `JR-AGENT-001`

| Topic | Default | Notes |
| --- | --- | --- |
| Canonical roster path | **`.cursor/agents/INDEX.md`** | Matches `.cursor/rules/` and `.cursor/orchestrations/` |
| Medium init | **Optional** roster-only INDEX | Contracts optional until orchestration enabled |
| Large init | **Required** INDEX before first orchestrated run | Contracts + orchestration `_template/` same init wave |
| Gate detail in INDEX | **High-level** gates 0–6 table only | Small-path skips, MG-* mapping → **`JR-ORCH-004`/`005`** + validation checklist |
| Test runners in INDEX | **Pointer** to target testing-strategy doc | No default runner names in Jarvis templates |
| Rule bindings in INDEX | **Placeholder table** by role | Target fills from `.cursor/rules/index.md` |
| Document precedence | ADRs → workflow/merge-ready rules → role contracts → templates → orchestration guide | Same order as [`task-folder-model.md`](../orchestration/task-folder-model.md) |
| WFD as source | Generalize roster **shape** only | No product domains, gap IDs, or package scripts in templates |

## Decisions recorded for `JR-AGENT-002`

| Topic | Default | Notes |
| --- | --- | --- |
| Canonical agent path | **`.cursor/agents/*.md`** | Same folder as INDEX; not repo-root `agents/` unless user directs |
| Medium init | **Optional** contracts | **Required** when copying `.cursor/orchestrations/_template/` or any active `PROJ-ORCH-*`; otherwise roster-only INDEX is enough |
| Large init | **Required** five contracts | Same session as INDEX + orchestration `_template/` |
| Gate detail in contracts | **Role-level** lifecycle 0–6 summary | Skip-path matrices stay in orchestration guide / **`JR-ORCH-005`** — not duplicated from WFD |
| Monorepo | **Single** repo-root `.cursor/agents/` | Per-package indexes only when user specifies |
| Commands in contracts | **Placeholder** → `docs/stack/commands.md` or README § Development | No invented test or lint commands in Jarvis templates |
| Minimum read sets | **Deferred** to **`JR-AGENT-003`** | Contracts list mandatory inputs; efficiency tables come later |
| Handoff directive blocks | **Deferred** to **`JR-AGENT-004`** | Orchestrator contract points to target orchestration guide |
| Template filenames | `*.example.md` under `docs/templates/universal-agents/` | Target drops `.example` suffix |

## Follow-on platform tasks

| ID | Outcome |
| --- | --- |
| `JR-AGENT-003` | Minimum read set tables per role (target INDEX or orchestration guide) |
| `JR-AGENT-004` | Next Agent Directive / handoff prompt templates for target orchestration guide |

## Related material

| Resource | Role |
| --- | --- |
| [`../templates/universal-agents/INDEX.example.md`](../templates/universal-agents/INDEX.example.md) | Roster template — links to five contract templates |
| [`../roadmap/backlog.md`](../roadmap/backlog.md) | `JR-AGENT-*` platform tasks |
| [`../roadmap/platform-spec.md`](../roadmap/platform-spec.md) | WFD concepts to generalize |
| WFD reference (conceptual) | Mature `.cursor/agents/` — do not link from target artifacts |
