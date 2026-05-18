# ADR Governance — REPLACE_WITH_PROJECT_NAME

## 1. Purpose

This document is the **authoritative process and policy** for Architectural Decision Records (ADRs) in this repository. Human contributors and AI agents must treat it as the single source of truth for ADR lifecycle, authority, conflicts, index maintenance, and how ADRs relate to agent rules and other project docs.

ADRs exist so architectural choices stay **remembered, traceable, and enforceable** across sessions and contributors—especially when agents lack durable memory. They record **significant** decisions that shape architecture, constrain future work, or replace a previously valid approach.

ADRs are **not** tutorials, style guides, or product marketing copy. They live in `adrs/` alongside [INDEX.md](INDEX.md) and [TEMPLATE.md](TEMPLATE.md): the index is the fast lookup; each ADR file is the detailed record; this file governs how all of that works.

---

## 2. What qualifies as an ADR

Write an ADR when a decision meets one or more of:

- It affects how a significant part of the codebase or system is structured or operated.
- It chooses between viable alternatives (rejected options still matter).
- It deprecates or replaces a pattern that was previously valid.
- It establishes a convention agents or developers must follow consistently.
- It would be non-obvious to a new contributor without explanation.
- Reversing it would require coordinated changes across files, services, or layers.

**Not every decision needs an ADR.** Formatting preferences, one-off bug fixes, and local implementation details belong in code, tests, or focused docs—not here.

**Heuristic:** _Would an independent implementer reliably reach the same conclusion without this record?_ If no, add an ADR.

---

## 3. ADR statuses

Every ADR has exactly one status, aligned with [INDEX.md](INDEX.md) and [TEMPLATE.md](TEMPLATE.md):

| Status | Meaning |
| --- | --- |
| **Proposed** | Under discussion or spike. **Not binding.** Do not implement as if finalized. |
| **Accepted** | Approved and in force. **Binding** for new and changed work unless a documented exception process applies. |
| **Deprecated** | No longer recommended; history remains. Legacy code may still match until migrated. |
| **Superseded** | Replaced by a newer ADR; must name the successor. **Not** valid guidance for new work—follow the successor. |

ADRs are **not deleted** from the index or folder; status changes preserve history.

**Note:** Some external ADR guides use **Active**. In this project, **Accepted** is the binding state.

---

## 4. Lifecycle

### 4.1 Proposing an ADR

Anyone (human or agent) may propose an ADR. A proposal must:

1. Copy [TEMPLATE.md](TEMPLATE.md), fill required sections, and remove instructional placeholder lines before review.
2. Use the next sequential number: `ADR-NNN-short-kebab-title.md` in `adrs/` (three-digit zero-padded `NNN`, never reused).
3. Set **Status** to **Proposed**.
4. Add a row to [INDEX.md](INDEX.md) with status **Proposed** and the correct **Domain**.

Agents must **surface** proposed ADRs for human review and must **not** treat **Proposed** as authoritative for implementation.

### 4.2 Review

Before **Accepted**, review should cover:

- Scope (not too broad or too narrow).
- Honest alternatives and consequences.
- Conflicts with existing **Accepted** ADRs.

### 4.3 Activation (explicit approval required)

An ADR becomes **Accepted** only when a **human explicitly approves** it (for example: “accept ADR-003” or “approve ADR-003” in the same thread or review). Then:

- Set **Status** to **Accepted** in the ADR file.
- Update the index row to **Accepted**.
- If needed, add or update agent rules (for example under `.cursor/rules/`) that enforce the decision.

Agents **may** perform the file and index updates when the user gives that explicit approval in the session. Agents **must not** promote an ADR from **Proposed** to **Accepted** without such approval (silence is not acceptance).

### 4.4 Deprecation vs supersession

- **Deprecated** — Retiring a pattern without a single named replacement ADR.
- **Superseded** — Another ADR explicitly replaces this one; cross-link successor fields and update both index rows.

---

## 5. Agent rules

### 5.1 Consult before deciding

Before making or changing **architectural** choices (patterns, libraries, boundaries, data ownership, security model), consult [INDEX.md](INDEX.md) for **Accepted** ADRs in the affected area.

### 5.2 Status defines authority

- **Accepted** → binding unless the user explicitly overrides after conflict surfacing (§5.4).
- **Proposed** → not binding; must not drive final implementation without human approval.
- **Deprecated** / **Superseded** → not valid for **new** decisions; legacy code may still match until migrated.

### 5.3 Agents may propose, not approve

When a gap exists, an agent may draft an ADR (**Proposed**), add it to the index, and notify the human. Agents may **Accept** only after **explicit** user approval in the same session (§4.3). Silence is not approval.

### 5.4 Surface conflicts

If instructions conflict with an **Accepted** ADR, stop, cite the ADR, summarize the decision, and ask how to proceed. Do not silently ignore either side.

### 5.5 Do not infer deprecation

Deprecation is only real when recorded as **Deprecated** or **Superseded** in an ADR and index. Do not infer it from comment age, file age, or low usage.

### 5.6 Scoped compliance

Respect task scope.

- **Disallowed:** Broad refactors solely for ADR cleanup when not part of the assigned task.
- **Allowed:** Small, low-risk compliance fixes **in files already being edited** for the task when local and supportive of the task goal.

**Escalate:** Multi-file or architectural migration → record an alignment gap (§8.3) or a `PROJ-*` / issue follow-up; do not expand scope unilaterally.

### 5.7 Partial implementation

An **Accepted** ADR may describe a target not yet fully realized. The ADR remains binding for **new and modified** work; existing gaps are debt, not a license to diverge on new work.

### 5.8 When no ADR exists

Use sound judgment, or propose a **Proposed** ADR when the choice is non-trivial, recurring, or architectural.

---

## 6. Domains and the index

[INDEX.md](INDEX.md) groups ADRs by **Domain** (default starter set: Platform, Architecture, Security, Quality, UI). When adding an ADR:

- Place the row in the correct domain table (create a table if you add a domain).
- Keep **Title**, **Status**, and **Description** in sync with the ADR file.

The ADR body uses **Scope** to define boundaries; the index row is the navigational projection—both must agree.

---

## 7. Authoring rules

### 7.1 Template

New ADRs follow [TEMPLATE.md](TEMPLATE.md). Required sections must be present unless genuinely N/A (do not drop required headings for convenience).

The **Orchestrated development** section is optional content: use it when the target project defines orchestration artifacts; otherwise state **Orchestration not required** per the template.

### 7.2 Language

- Prefer clear, testable statements in **Decision**.
- **Context** may use past tense; decision and consequences should be precise.
- Name concrete libraries, boundaries, and artifacts where they matter.

### 7.3 Alternatives

List real alternatives that were considered; weak straw options undermine the record.

### 7.4 Numbering and filenames

`ADR-NNN-short-kebab-title.md` under `adrs/`; `NNN` sequential, never reused.

### 7.5 Linking

Cross-link **Superseded** / successor ADRs. Use explicit references when decisions constrain each other without supersession.

---

## 8. Relationship to other artifacts

### 8.1 ADRs and agent rules

- ADRs record **why** and **what**; agent rules (for example `.cursor/rules/`) help apply constraints in daily work.
- An **Accepted** ADR **may** motivate a rule; a rule that contradicts an **Accepted** ADR is wrong and should be fixed—the ADR wins.
- Rules may cite ADR ids for traceability.

### 8.2 README

The root README summarizes intent and routes to `adrs/INDEX.md`. It must not contradict **Accepted** ADRs. When README and ADRs diverge, fix README or ADR via human decision and track interim drift if needed (§8.3).

### 8.3 Alignment gaps

When implementation differs from an **Accepted** ADR and the gap is not fixed in the same change, record it in the project alignment gaps doc (default: `docs/adr-alignment-gaps.md`). Include owner, severity, affected areas, and remediation. Do not hide drift only in code comments.

### 8.4 Project roadmap

Setup tasks (`PROJ-*` or equivalent under `docs/roadmap/`) track scaffolding and follow-ups; ADRs record durable decisions. Completing a `PROJ-ADR-*` task does not by itself **Accept** an ADR.

---

## 9. Index maintenance

[INDEX.md](INDEX.md) is the canonical list of ADRs, statuses, and short descriptions.

- Update the index in the **same change** as any new ADR or status transition.
- Never leave an ADR file and its index row inconsistent.
- Prefer concise index rows; depth lives in the ADR file.

If index and ADR disagree, fix immediately; the ADR file wins for detailed meaning, but the index must match **Status**, **Title**, and **Domain**.

---

## 10. Orchestrated development (optional)

Use when the project defines a Plan–Build–Test–Validate (or similar) workflow with task folders, manifests, and validation artifacts. If the project has no orchestration docs yet, omit detailed fills under [TEMPLATE.md](TEMPLATE.md) **Orchestrated development** and state **Orchestration not required**.

When orchestration exists, policy for gates and merge-ready checks belongs in the project's orchestration guide and validation checklist—not duplicated here. ADRs should link to those artifacts rather than embedding project-specific gate IDs unless this ADR itself defines a process decision.

**Precedence when documents conflict:** **Accepted** ADRs and this governance doc → project workflow / merge-ready rules → agent role contracts → orchestration templates.

---

## 11. Bootstrap note

This governance document is a foundational policy artifact; it does not need its own ADR to exist. **Material** process changes here should go through human review and, when they encode new architecture or constraints, may warrant a new ADR.

---

_Last updated: replace when materially edited._
