# Universal documentation conventions scaffold

Generic documentation conventions for target projects: production source comments, tests, architecture writeups (`docs/architecture/`), and user-facing guides (`docs/guides/`). Jarvis copies and adapts these files into the target repository.

**Platform task:** `JR-UNIVERSAL-003`  
**Copy source:** [`../templates/universal-docs/`](../templates/universal-docs/)  
**Target layout:**

| Path | Role |
| --- | --- |
| `docs/documentation-conventions.md` | Canonical policy (from `documentation-conventions.md` template) |
| `docs/architecture/README.md` | Index for architecture topics (from `architecture-README.md`) |
| `docs/guides/README.md` | Index for user-facing guides (from `guides-README.md`; create when product has external users) |
| `docs/stack/source-documentation.md` | Stack-specific comment syntax (from `stack-source-documentation.example.md`; required when production code exists) |

**No Cursor rule** for this scaffold (confirmed): agents load policy via the root README Documentation map and this file. Projects may add a topic rule later under `PROJ-RULE-*` if enforcement in `.cursor/rules/` is desired ([`universal-rules`](../universal-rules/README.md)).

Jarvis authoring workflow for README-driven doc creation stays in [`../target-readme/`](../target-readme/) — not copied to targets.

## When to scaffold

| Initialization path | Documentation conventions |
| --- | --- |
| **Small** | Optional — copy `docs/documentation-conventions.md` when the repo will hold production code or multiple agents edit docs |
| **Medium** | Default — conventions doc + `docs/architecture/README.md`; add `docs/guides/README.md` when README describes external users |
| **Large** | Required — same as medium; create first architecture topic from README § Boundaries; fill stack source doc from verified manifests |

See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md).

## Copy and adapt (agents)

1. If the target already has `docs/documentation-conventions.md`, **audit** — merge missing sections; do not replace team-specific policy without user approval.
2. Copy [`documentation-conventions.md`](../templates/universal-docs/documentation-conventions.md) → `docs/documentation-conventions.md`.
3. Replace `REPLACE_WITH_PROJECT_NAME`.
4. Copy [`architecture-README.md`](../templates/universal-docs/architecture-README.md) → `docs/architecture/README.md` (create `docs/architecture/` if missing).
5. When the README implies end users or operators, copy [`guides-README.md`](../templates/universal-docs/guides-README.md) → `docs/guides/README.md`.
6. When production code exists, copy [`stack-source-documentation.example.md`](../templates/universal-docs/stack-source-documentation.example.md) → `docs/stack/source-documentation.md` and fill from **verified** stack (README § Stack, lockfiles, manifests). Remove inapplicable sections.
7. Add one line to root README § Documentation for each path above.
8. Add or update `PROJ-DOC-*` rows in `docs/roadmap/backlog.md` with evidence paths.

**Handoff rule:** Generated docs must not link to the Jarvis repository. All `see` targets must be target-project paths.

## Relationship to other Jarvis scaffolds

| Scaffold | Relationship |
| --- | --- |
| [`../universal-readme/README.md`](../universal-readme/README.md) | README governance; routes contributor docs vs root README |
| [`../universal-adr/README.md`](../universal-adr/README.md) | ADRs own decisions; architecture docs illustrate |
| [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) | README signals → `PROJ-DOC-*` and doc paths |
| [`../stack-scaffolding/README.md`](../stack-scaffolding/README.md) | Stack detection/confirmation (`JR-STACK-001`); fills `docs/stack/stack-profile.md` and `docs/stack/source-documentation.md` |
| [`../universal-rules/README.md`](../universal-rules/README.md) | Rules index may reference this doc; this scaffold does not ship a default rule |

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- Replacing an existing `docs/documentation-conventions.md` that encodes team-specific standards (for example monorepo-wide doc platforms).
- Removing `docs/architecture/` or `docs/guides/` when **Accepted** ADRs or the README Documentation map reference them.
- Weakening universal exemptions (for example requiring full API docs on every test case) without an ADR or explicit user direction.
- Adding a **required** always-apply Cursor rule for documentation when the project chose **doc-only** loading (offer `PROJ-RULE-*` instead).

Routine copy, placeholder replacement, and empty architecture/guides indexes do not require extra approval.

## Decisions recorded for `JR-UNIVERSAL-003`

Defaults favor long-term agent efficiency; override per target when the user directs:

| Topic | Default |
| --- | --- |
| Canonical policy path | `docs/documentation-conventions.md` |
| Cursor rule | **None** (doc-only; user confirmed for platform scaffold) |
| Architecture layout | `docs/architecture/` + `README.md` index + topic files |
| User-facing layout | `docs/guides/` for end-user/operator guides |
| Stack comment syntax | `docs/stack/source-documentation.md` from example template when code exists |
| Production doc principles | Language-agnostic §2 in conventions; stack file owns syntax |
| Tests | Describe suite scope and shared helpers; light per-case docs |
| Setup vs product docs | `docs/roadmap/` is planning only; not covered by conventions body |
