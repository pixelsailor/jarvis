# Universal ADR scaffold

Generic Architecture Decision Record (ADR) layout for target projects. Jarvis copies and adapts these files into the target repository's `adrs/` folder at the repository root.

**Platform task:** `JR-UNIVERSAL-001`  
**Copy source:** [`../templates/universal-adr/`](../templates/universal-adr/)  
**Target layout:** `adrs/INDEX.md`, `adrs/GOVERNANCE.md`, `adrs/TEMPLATE.md`, then `adrs/ADR-NNN-*.md` as decisions are recorded.

## When to scaffold

| Initialization path | ADR scaffold |
| --- | --- |
| **Small** | Optional — only if README states non-negotiable architectural boundaries |
| **Medium** | Default — index, governance, template; first **Accepted** ADR from README boundaries |
| **Large** | Required — same as medium; orchestration-heavy ADRs use optional template section when orchestration docs exist |

See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md).

## Copy and adapt (agents)

1. If the target already has `adrs/`, **audit** — do not overwrite Accepted ADRs or a working index. Merge governance only when missing or when the user approves a refresh.
2. Copy all files from [`../templates/universal-adr/`](../templates/universal-adr/) into `adrs/`.
3. Replace every `REPLACE_WITH_PROJECT_NAME` placeholder in `GOVERNANCE.md` and the index agent block.
4. **Customize domains** in `INDEX.md` (see [Domain taxonomy](#domain-taxonomy)) to match the target README and stack.
5. Add the first **Proposed** or **Accepted** ADR from README principles/boundaries (human approves **Accepted**).
6. Link from the target README Documentation section: `adrs/INDEX.md` (one line; no ADR bodies in README).
7. Add or update `PROJ-ADR-*` rows in `docs/roadmap/backlog.md` with evidence paths.

**Handoff rule:** Generated ADRs must not link to the Jarvis repository. All `see` targets must be target-project paths.

## Domain taxonomy

The template index ships with a **starter** set of domain tables. Agents should **rename, merge, or add** domains so the index matches how the project discusses architecture (typically aligned with README § Boundaries and stack).

| Starter domain | Typical contents |
| --- | --- |
| **Platform** | Product operating model, deployment topology, environments, feature envelopes |
| **Architecture** | Layering, module boundaries, data flow, storage authority, sync, major patterns |
| **Security** | AuthN/Z, secrets, trust boundaries, threat-related choices |
| **Quality** | Testing strategy, observability, performance budgets, contract validation |
| **UI** | Components, design system, accessibility, client interaction patterns |

**Default set (confirmed):** Platform, Architecture, Security, Quality, UI. Add domains (for example **Data**, **Integration**, **Operations**) only when the project routinely files ADRs in that area—prefer fewer, stable domains over many sparse tables.

## Optional alignment gaps doc

When implementation drifts from **Accepted** ADRs and the gap is not fixed in the same change, record it in a project-owned tracking doc (not in `adrs/`).

- **Default path:** `docs/adr-alignment-gaps.md`
- **Starter:** [`../templates/universal-adr/alignment-gaps.example.md`](../templates/universal-adr/alignment-gaps.example.md)

Use a different path only when the target already has an equivalent; link it from README or `GOVERNANCE.md` §8.

## Relationship to other Jarvis scaffolds

| Scaffold | Relationship |
| --- | --- |
| [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md) | README signals → `PROJ-ADR-*` and first ADRs |
| [`../target-roadmap/handoff.md`](../target-roadmap/handoff.md) | Medium/large paths expect `adrs/` index + governance |
| `JR-UNIVERSAL-004` (future) | Cursor rules cite **Accepted** ADRs; ADRs do not duplicate rule text |
| `JR-ORCH-*` (future) | Orchestration block in `TEMPLATE.md` activates when target has orchestration docs |

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- Overwriting an existing target `adrs/` tree that already contains **Accepted** records.
- Changing domain taxonomy when **Accepted** ADRs are already filed under the old domain names (needs migration plan).
- Promoting any ADR from **Proposed** to **Accepted** without explicit user approval in the session (agents may draft; **Accept** only on direct user instruction).
- Removing or renumbering `ADR-NNN` files that other docs, rules, or `PROJ-*` tasks reference.

Routine copy, placeholder replacement, and **Proposed** ADR drafts do not require extra approval.

## Decisions recorded for `JR-UNIVERSAL-001`

These defaults favor long-term agent efficiency; override per target when the user directs:

| Topic | Default |
| --- | --- |
| ADR location | Repository root `adrs/` |
| Filename pattern | `ADR-NNN-short-kebab-title.md` (three-digit `NNN`, never reused) |
| Binding status | Only **Accepted**; agents may **Accept** only when the user explicitly approves that ADR in the same thread (for example “accept ADR-003”) |
| Index authority | `INDEX.md` + `GOVERNANCE.md` + per-file ADR; index updated in same session as status changes |
| Orchestration in template | Optional section; fill when target orchestration exists (`JR-ORCH-*`) |
| Alignment gaps | Optional `docs/adr-alignment-gaps.md`; not mandatory at handoff for small path |
