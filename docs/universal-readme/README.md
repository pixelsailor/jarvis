# Universal README governance scaffold

Generic README governance for target projects: what belongs in the root README, where detail lives, and how to maintain alignment with ADRs and the setup backlog. Jarvis copies and adapts these files into the target repository.

**Platform task:** `JR-UNIVERSAL-002`  
**Copy source:** [`../templates/universal-readme/`](../templates/universal-readme/)  
**Target layout:**

| Path | Role |
| --- | --- |
| `docs/readme-governance.md` | Canonical human- and agent-readable policy (from `GOVERNANCE.md` template) |
| `.cursor/rules/readme-governance.mdc` | **Medium/large:** required with the doc. **Small:** optional. `globs: README.md`, points to the doc above |

**Jarvis authoring workflow** (outline, intake, audit) stays in [`../target-readme/`](../target-readme/) — not copied to targets. Targets own **governance** after handoff.

## When to scaffold

| Initialization path | README governance |
| --- | --- |
| **Small** | Optional — copy `docs/readme-governance.md` only if README is expected to grow or multiple agents will edit it |
| **Medium** | Default — `docs/readme-governance.md` + `.cursor/rules/readme-governance.mdc` |
| **Large** | Required — same as medium; link both from root README § Documentation |

See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md).

## Copy and adapt (agents)

1. If the target already has `docs/readme-governance.md` or a `.cursor/rules/readme-governance.mdc`, **audit** — merge missing sections; do not overwrite project-specific policy without user approval.
2. Copy [`GOVERNANCE.md`](../templates/universal-readme/GOVERNANCE.md) → `docs/readme-governance.md`.
3. Replace `REPLACE_WITH_PROJECT_NAME` in the title and footer.
4. Adjust §2 section table only when the target README uses a deliberate, documented alternate outline (record why in a `PROJ-DOC-*` note).
5. Copy [`readme-governance.mdc`](../templates/universal-readme/readme-governance.mdc) → `.cursor/rules/readme-governance.mdc` on medium/large paths (fix relative link to `docs/readme-governance.md` if rules live in a non-default folder).
6. Add one line to root README § Documentation pointing to `docs/readme-governance.md`.
7. Add or update `PROJ-DOC-*` / `PROJ-RULE-*` rows in `docs/roadmap/backlog.md` with evidence paths.

**Handoff rule:** Governance files must not link to the Jarvis repository. All `see` targets must be target-project paths.

## Relationship to other Jarvis scaffolds

| Scaffold | Relationship |
| --- | --- |
| [`../target-readme/outline.md`](../target-readme/outline.md) | **Authoring** shape for the README body; governance **maintains** that shape after init |
| [`../target-readme/handoff-checklist.md`](../target-readme/handoff-checklist.md) | README self-containment checks before `PROJ-README-*` complete |
| [`../target-roadmap/readme-sync.md`](../target-roadmap/readme-sync.md) | Material README edits → `PROJ-*` backlog (copy with target roadmap workflow) |
| [`../universal-adr/README.md`](../universal-adr/README.md) | ADRs own decisions; README summarizes — no duplication |
| [`../universal-docs/README.md`](../universal-docs/README.md) | Source, test, architecture, and user-guide doc policy; README routes here |
| `JR-UNIVERSAL-004` (future) | Rules index cites this governance doc; governance does not duplicate rule bodies |

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- Replacing an existing `docs/readme-governance.md` that encodes team-specific policy (for example monorepo or docs-site exceptions).
- Removing standard README sections that downstream **Accepted** ADRs or compliance docs reference by heading name.
- Choosing **doc-only** vs **doc + Cursor rule** when the user has already standardized on one pattern in `.cursor/rules/`.
- Weakening "no Jarvis dependency" or "verified commands only" to add unverified tooling marketing copy to the README.

Routine copy, placeholder replacement, and Documentation map links do not require extra approval.

## Decisions recorded for `JR-UNIVERSAL-002`

Defaults favor long-term agent efficiency; override per target when the user directs:

| Topic | Default |
| --- | --- |
| Canonical policy path | `docs/readme-governance.md` |
| Cursor rule | `.cursor/rules/readme-governance.mdc`, `globs: README.md`, `alwaysApply: false` |
| Medium/large scaffold | **Doc + rule** (confirmed platform default); small path may use doc only |
| Heading contract | Align with [`outline.md`](../target-readme/outline.md) required/conditional sections |
| README ↔ backlog sync | Defer to copied `readme-sync.md` when `docs/roadmap/` exists; §6 in governance summarizes material vs immaterial |
| ADR drift doc | `docs/adr-alignment-gaps.md` per universal ADR scaffold |
| WFD pattern source | Routing table and 10,000-foot discipline generalized — no WFD product identifiers in templates |
