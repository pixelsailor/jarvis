# ADR-NNN: REPLACE_WITH_SHORT_TITLE

Copy this file to `adrs/ADR-NNN-short-kebab-title.md` at the repository root, incrementing the numeric segment `NNN` (zero-pad to three digits, e.g. `ADR-001`, `ADR-002`) from the highest existing `ADR-*` file in `adrs/`. Replace placeholders (including `REPLACE_WITH_*`) and remove instructional lines before review. Use an imperative, short title (for example: “Store authoritative user data in the application database”).

## Status

One of: **Proposed** · **Accepted** · **Deprecated** · **Superseded**

Meaning, lifecycle, and agent treatment of each status are defined in [GOVERNANCE.md](GOVERNANCE.md) (§3–5). In short: **Proposed** is not binding; **Accepted** is binding for new and changed work; **Deprecated** / **Superseded** are not valid guidance for new decisions—follow any successor ADR linked in **Supersession notes**.

## Date

YYYY-MM-DD (date of last material change to this ADR)

## Scope

Define **where this decision applies** and **where it explicitly does not**. Prevents silent scope creep and duplicate ADRs.

- **In scope:** systems, directories, features, data lifecycles, or roles governed by this ADR.
- **Out of scope:** adjacent concerns governed by other ADRs or non-architecture docs.

## Context

Describe the forces driving a decision: product constraints, technical constraints, prior art, and what goes wrong if nothing is decided.

### Decision pressure (required)

What pressure **forces a decision now**? Be specific (for example: scaling limit, compliance deadline, ambiguous ownership, incident recurrence, coupling that blocks delivery). If there is no real pressure, reconsider whether an ADR is needed yet.

### Supporting context

- What problem or ambiguity are we resolving?
- What options were considered at a high level (even if briefly rejected)?
- What project principles or boundaries must stay true (see root README)?

## Decision

State the decision in clear, testable language. Prefer “we will …” / “we will not …” over vague principles.

- Single coherent decision (split additional choices into separate ADRs when they can evolve independently).

### Explicit exclusions (required)

List **tempting alternatives we are not choosing** and why (briefly). If a reader might assume “we could also just …,” name it here and mark it out of scope.

## Consequences

### Positive

Benefits: simplicity, safety, velocity, operability, alignment with stack.

### Negative

Tradeoffs, costs, or constraints the team accepts.

### Risks and mitigations

Known risks and how we detect or reduce them (monitoring, tests, follow-up ADRs).

## Operational impact

Effects on **performance**, **cost**, **debugging**, **deployment**, or **developer workflow**. Use “none material” only when genuinely negligible.

## Examples (optional)

Concrete examples of how this decision appears in code, data, or flows.

- …

Remove this section entirely when empty.

## Compliance (optional)

Relevant standards, frameworks, or regulatory requirements this decision satisfies, aligns with, or intentionally defers.

- **Standards:** e.g. WCAG 2.1 Level AA, OWASP ASVS — or **Not applicable** / **None**.

Remove this section entirely when nothing applies.

## Notes (optional)

External references, links to prior discussions, or historical context.

- …

Remove this section entirely when empty.

## Enforcement rules

Per-ADR content only (how **this** decision is enforced). Policy for ADRs ↔ agent rules and index sync is in [GOVERNANCE.md](GOVERNANCE.md) (§5 and §8).

- **Agent rules:** Which rule files or checks should reflect this ADR (or where new rules belong).
- **Code / architecture:** Directories, boundaries, or patterns that must conform.
- **When to revisit:** Triggers that require updating this ADR instead of silently diverging.

## Supersession notes

- **When to supersede:** Open a **new** ADR when the **core invariant or boundary** of this decision changes.
- **Stable identifiers:** Keep the ADR number fixed; revise content in place until the decision is fundamentally replaced.
- **If superseded:** Link `ADR-NNN-short-kebab-title.md` and summarize what changed.
- **If partially obsolete:** Prefer a new ADR for the new decision and narrow this ADR’s scope.

---

## Orchestrated development

Use this section when work is **non-trivial**, **multi-phase**, or touches **durable architecture**. For a one-line doc fix or an isolated bug with no architectural implication, write **Orchestration not required** on the first line under this heading and **omit the subsections** below.

Policy for when orchestration applies lives in [GOVERNANCE.md](GOVERNANCE.md) §10 and the project's orchestration guide (if any).

### When orchestration is required

Treat Plan–Build–Test–Validate (or equivalent) orchestration as **required** when any of the following hold; otherwise keep this ADR but **omit detailed fills** below (still keep the heading and **Orchestration not required** if nothing applies):

- The change spans multiple PRs or phases, or has significant rollback risk.
- The change touches ADR-governed boundaries named in **Enforcement rules**.
- You would otherwise need a written plan, validation report, and test evidence before calling work merge-ready.

### Authoritative workflow artifacts

Link the project's orchestration entrypoint (for example `.cursor/agents/orchestrator.md`, `docs/ORCHESTRATED_DEVELOPMENT.md`, or equivalent). **Omit until the project defines these paths.**

- Orchestration guide: `REPLACE_WITH_ORCHESTRATION_GUIDE_PATH`
- Task folder pattern: `REPLACE_WITH_TASK_FOLDER_PATTERN` (for example `.cursor/orchestrations/{task-id}/`)

### Agent and coding conventions

Minimum read set for implementers (target-project paths only):

- Agent rules index: `REPLACE_WITH_RULES_INDEX_PATH` (for example `.cursor/rules/index.md`)
- Project best-practices rule or doc: `REPLACE_WITH_BEST_PRACTICES_PATH`

### Lint / automated style

Point to the project's lint or format config if architectural work must pass defined checks. **Omit if not yet defined.**

- Config: `REPLACE_WITH_LINT_CONFIG_PATH`

### Relevant ADRs for implementation

List ADR numbers and titles the **Planner** must read before drafting a plan for work in this area. State **implication**, not pointer-only text.

- ADR-…: …

### Planning artifact

Link or path to the scoped plan. **Omit until a plan exists.**

- Plan: `REPLACE_WITH_PLAN_PATH`

### Builder scope boundary

One bounded phase or PR-sized slice this ADR governs for the **current** change set, and what is explicitly **out of scope**.

### Validator expectations

What the **Validator** must verify against **this ADR**, applicable agent rules, and the project alignment gaps doc (if any).

### Tester role and evidence

What the **Tester** should add or update; cite **verified** commands from the project README or package scripts—do not invent commands.

### Alignment gaps

If current implementation differs from this ADR and the gap is not fixed in the same change, record it in `docs/adr-alignment-gaps.md` (or the project's equivalent). Do not hide divergence only in code comments.

### Merge / workflow gates

Confirm before claiming merge-ready when orchestration applies. Link the project's validation checklist or merge-ready rules. **Omit gate IDs until the project defines them.**

- [ ] ADR created or updated **before** durable architecture change (or explicit follow-up exists).
- [ ] Known deviations documented as alignment gaps when not fixed in scope.
- [ ] Validation evidence recorded for behavior this ADR cares about.
- [ ] Test evidence recorded when orchestration applies.
