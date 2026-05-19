# README → downstream scaffolding map

The target README is the **control document** for what Jarvis (or a follow-on agent) creates next. This map links README content to `PROJ-*` backlog areas, artifacts, and read order — without copying WFD product or stack defaults.

**Prerequisite:** A draft or audited README exists ([`outline.md`](./outline.md), [`audit.md`](./audit.md)).

## Initialization paths

Choose once per project (intake Q7). When unclear, default to **medium** and tell the user.

| Path | README work | Typical `PROJ-*` scope | Orchestration |
| --- | --- | --- | --- |
| **Small** | Full required outline; minimal Documentation | `PROJ-README`, `PROJ-RULE` (1–2 rules), `PROJ-HANDOFF` | No task folders |
| **Medium** | + boundaries refined | + `PROJ-ADR`, `PROJ-DOC`, `PROJ-STACK` | Optional for large doc sets |
| **Large** | + roadmap direction, structure | + `PROJ-AGENT`, `PROJ-ORCH`, validation docs | Task-folder model — [`../orchestration/`](../orchestration/) (`JR-ORCH-001`–`003`) |

## Signal → backlog → artifacts

| README signal | Read first | Backlog prefix | Artifacts to create or link |
| --- | --- | --- | --- |
| Principles, non-negotiables | README § Principles | `PROJ-ADR-*` | `adrs/INDEX.md`, `GOVERNANCE.md`, `TEMPLATE.md`; Accepted ADRs for each non-negotiable that affects implementation |
| Core capabilities | README § Capabilities | `PROJ-DOC-*` | Topic docs under `docs/` when a capability needs flow/architecture prose |
| Technology stack | README § Stack + [`stack-profile.md`](../stack-scaffolding/confirmation.md#recording-target-repository) + repo files | `PROJ-STACK-*` | Detect/confirm per [`stack-scaffolding/`](../stack-scaffolding/README.md); then [`commands.md`](../stack-scaffolding/commands.md), [`testing.md`](../stack-scaffolding/testing.md), [`runtime.md`](../stack-scaffolding/runtime.md), [`dependencies.md`](../stack-scaffolding/dependencies.md), rules, `docs/stack/` — **verified from repo** |
| Architecture boundaries | README § Boundaries | `PROJ-ADR-*`, `PROJ-RULE-*` | ADRs for client/server, secrets, data authority; always-apply rules that enforce boundaries |
| Data ownership | README § Data ownership | `PROJ-ADR-*` | ADR for storage/sync authority; rules for local vs cloud paths |
| Documentation map entries | README § Documentation | `PROJ-DOC-*`, `PROJ-RULE-*` | Each linked path must exist or have an open `PROJ-*` task |
| Development commands | README § Development + `docs/stack/commands.md` | `PROJ-STACK-001` | [`commands.md`](../stack-scaffolding/commands.md); no parallel cheat sheet; rules may cite same commands |
| Testing layers | `docs/stack/testing-strategy.md` | `PROJ-STACK-003` | [`testing.md`](../stack-scaffolding/testing.md); commands from `commands.md` only |
| Runtime / secrets | README § Boundaries + `docs/stack/runtime-boundaries.md` | `PROJ-STACK-004` | [`runtime.md`](../stack-scaffolding/runtime.md); env **names** only |
| Dependencies / tooling | `docs/stack/dependency-review.md` (optional) | `PROJ-STACK-005` | [`dependencies.md`](../stack-scaffolding/dependencies.md); no automatic upgrades |
| Roadmap direction (product themes) | README § Roadmap | `PROJ-DOC-*` | Product docs — **not** the same as `docs/roadmap/backlog.md` setup tasks |
| Setup in progress | README links `docs/roadmap/` | `PROJ-README-*`, all areas | [`templates/target-project-roadmap/`](../../templates/target-project-roadmap/) |

## Universal vs stack-specific

| Layer | Driven by | Jarvis source (copy/adapt into target) |
| --- | --- | --- |
| **Universal** | Any initialized project | [`docs/universal-adr/README.md`](../universal-adr/README.md) (ADR layout); [`docs/universal-readme/README.md`](../universal-readme/README.md) (README governance); [`docs/universal-docs/README.md`](../universal-docs/README.md) (doc conventions); [`docs/universal-rules/README.md`](../universal-rules/README.md) (Cursor rules layout + index); [`docs/universal-pr-commit/README.md`](../universal-pr-commit/README.md) (PR/commit guide + GitHub template); [`docs/universal-validation/README.md`](../universal-validation/README.md) (validation checklist + optional rule); [`docs/universal-handoff/README.md`](../universal-handoff/README.md) (handoff self-containment audit); [`docs/universal-agents/README.md`](../universal-agents/README.md) (agent INDEX + role contracts — large path; INDEX optional on medium) |
| **Stack-specific** | README stack + `docs/stack/stack-profile.md` + detected files | [`stack-scaffolding/`](../stack-scaffolding/README.md) — detect/confirm then [`selection.md`](../stack-scaffolding/selection.md) + [`upstream-capabilities.md`](../stack-scaffolding/upstream-capabilities.md); rules and upstream refs **authored in target**, not copied from Jarvis |

**Rule:** Generated files live in the target repo. Jarvis repository paths are not valid `see` targets in target rules or ADRs.

## Agent read sets (by role)

Minimize cold-start token load:

| Role | Minimum read set |
| --- | --- |
| **Any agent** | Target `README.md` → `docs/roadmap/README.md` → `docs/roadmap/backlog.md` |
| **Planner** | + `adrs/INDEX.md` (if exists) + open `PROJ-*` dependencies |
| **Builder** | + relevant Accepted ADR + `.cursor/rules/index.md` + topic doc for the task |
| **Validator** | + validation checklist doc + `PROJ-HANDOFF-*` open items |

## When README changes → update backlog

**Canonical workflow:** [`../target-roadmap/readme-sync.md`](../target-roadmap/readme-sync.md) (material vs immaterial edits, add/reopen/defer/cancel, stale artifacts, roadmap index sync).

Summary: after any **material** README edit, update `docs/roadmap/backlog.md` in the same session — do not rely on chat memory. Re-run [`audit.md`](./audit.md) rubric when principles, stack, or boundaries change.

| README change | Typical new/updated tasks |
| --- | --- |
| New non-negotiable | `PROJ-ADR-*` Accept decision; `PROJ-RULE-*` if enforceable in agent rules |
| Stack component added/removed | `PROJ-STACK-*`; update Development section with evidence |
| New capability | `PROJ-DOC-*` or product doc; optional ADR if architectural |
| Boundary tightened (e.g. secrets) | `PROJ-ADR-*` + `PROJ-RULE-*` |
| Documentation map link added | `PROJ-DOC-*` until file exists |
| Handoff claimed in README | Run [`handoff.md`](../target-roadmap/handoff.md) — do not close backlog from README prose alone |

## Scaffolding order (default)

```text
1. README (draft or audit-complete)
2. docs/roadmap/ (README + backlog with PROJ-* from this map)
3. adrs/ index + governance + first Accepted ADRs from boundaries
4. .cursor/rules/ index + always-apply + topic rules
5. docs/readme-governance.md (+ optional readme-governance rule) + docs/documentation-conventions.md + docs/pr-and-commit-guide.md (+ .github/pull_request_template.md on GitHub medium/large) + docs/validation-checklist.md (medium/large default) + docs/architecture/ (+ docs/guides/ when applicable) + topic docs referenced from README
6. `.cursor/orchestrations/_template/` + orchestration guide + `.cursor/agents/INDEX.md` + five role contracts (large path; from [`../universal-agents/README.md`](../universal-agents/README.md)) — [`../orchestration/README.md`](../orchestration/README.md)
7. docs/handoff-self-containment.md + PROJ-HANDOFF-* verification
```

Do not generate step 6 before step 3 when boundaries are still Weak in the audit.

## WFD patterns to generalize (not copy)

From WFD-style discipline, without WFD identifiers:

- Task folders for **large** init only
- Manifest-owned resumability
- Separate lifecycle gates vs merge-ready / handoff checks
- Evidence on backlog completion

See [`platform-spec.md`](../roadmap/platform-spec.md#wfd-concepts-to-generalize).

## Open platform decisions

These affect how many rows this map spawns:

- Mandatory universal scaffolds for every target ([`open-decisions.md`](../roadmap/open-decisions.md))
- Infer vs ask thresholds (non-stack fields; stack language/framework: [`stack-scaffolding/detection.md`](../stack-scaffolding/detection.md#confidence))
Until closed, **medium path** table above is the default.
