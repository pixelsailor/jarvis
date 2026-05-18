# Universal handoff self-containment scaffold

Repo-wide **handoff verification** for target projects: confirm generated artifacts do not rely on the Jarvis repository, sibling workspace paths, or chat-only context. Complements README-only checks and `PROJ-HANDOFF-*` backlog gates.

**Platform task:** `JR-UNIVERSAL-007`  
**Copy source:** [`../templates/universal-handoff/`](../templates/universal-handoff/)  
**Target layout:**

| Path | Role |
| --- | --- |
| `docs/handoff-self-containment.md` | Canonical independence audit (from `handoff-self-containment.md` template) |

**Jarvis workflow (not copied to targets):** When to declare initialization complete — [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md). README layer — [`../target-readme/handoff-checklist.md`](../target-readme/handoff-checklist.md).

## When to scaffold

| Initialization path | Handoff self-containment doc |
| --- | --- |
| **Small** | Default — copy before `PROJ-HANDOFF-*`; short scan is enough for minimal scaffolds |
| **Medium** | Required — run full checklist before `PROJ-HANDOFF-001` / `PROJ-HANDOFF-002` |
| **Large** | Required — same; re-run after orchestration or agent scaffolds land |

See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) (scaffolding step 7).

## Copy and adapt (agents)

1. Copy [`handoff-self-containment.md`](../templates/universal-handoff/handoff-self-containment.md) → `docs/handoff-self-containment.md`.
2. Replace `REPLACE_WITH_PROJECT_NAME` once at the top.
3. Ensure `PROJ-HANDOFF-001` and `PROJ-HANDOFF-002` in `docs/roadmap/backlog.md` reference this doc (see target backlog template).
4. Run the checklist **before** marking those tasks `[x]`; record `Evidence:` sub-bullets per [`../target-roadmap/conventions.md`](../target-roadmap/conventions.md).
5. Link from root README § Documentation **during init only** (optional) or from `docs/roadmap/README.md`; trim root README link after handoff if the team prefers a slimmer map.
6. Do **not** add Jarvis repository URLs to this file after copy — it describes what to verify, not where Jarvis lives.

**Handoff rule:** After copy, the target project must operate without reading Jarvis docs. This file is **target-owned** verification guidance.

## Relationship to other artifacts

| Artifact | Scope |
| --- | --- |
| [`../target-readme/handoff-checklist.md`](../target-readme/handoff-checklist.md) | Layer 1 — root `README.md` only |
| **This scaffold** (`docs/handoff-self-containment.md`) | Layer 2 evidence — whole-repo independence (`PROJ-HANDOFF-001`, `PROJ-HANDOFF-002`) |
| [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | Layer 3 — required `PROJ-*`, user sign-off, when Jarvis may claim handoff |
| `PROJ-HANDOFF-003` | Human acknowledgment — not replaceable by this checklist |

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- Marking `PROJ-HANDOFF-001` / `PROJ-HANDOFF-002` complete when scan finds Jarvis links the user insists on keeping (for example monorepo dev layout).
- Declaring **partial handoff** with known Jarvis dependencies — see [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md#partial-handoff-exception).
- Removing provenance notes the user wants in `CONTRIBUTING.md` or `docs/roadmap/` history.

Routine copy, scan, fix broken links, and evidence sub-bullets do not require extra approval.

## Decisions recorded for `JR-UNIVERSAL-007`

Defaults favor long-term agent efficiency; override per target when the user directs:

| Topic | Default |
| --- | --- |
| Canonical path | `docs/handoff-self-containment.md` |
| Cursor rule | **None** — doc-only; referenced from `PROJ-HANDOFF-*` and roadmap |
| Filename | Avoid `jarvis-*` in target paths — doc is about independence, not a runtime dependency on Jarvis |
| Scan patterns | Include `rg`/search hints in template for agent repeatability |
| Provenance | Historical Jarvis credit allowed in `docs/roadmap/` or CONTRIBUTING — **not** in root README as required reading |
| Overlap with `JR-VALIDATION-004` | This doc is the **operational checklist**; validation backlog task may add evidence tiers later |
