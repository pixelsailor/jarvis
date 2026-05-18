# ADR Index — REPLACE_WITH_PROJECT_NAME

## Agent instructions

- Process, status meanings, conflicts, and index sync rules: [GOVERNANCE.md](GOVERNANCE.md).
- Consult this index before making any **architectural** decision.
- Only **Accepted** ADRs are binding; do not act on **Proposed**, **Deprecated**, or **Superseded** entries.
- **Superseded** entries must name a successor in the ADR body and index — follow the successor, not the old record.
- If no relevant **Accepted** ADR exists for a decision you are about to make, draft one with status **Proposed** and surface it for review before treating the choice as final.
- ADRs are grouped by **Domain** in the category tables below. Customize domain names when copying this scaffold (see [GOVERNANCE.md](GOVERNANCE.md) §6).

## Index maintenance rules

- Update this index in the **same session** as any ADR status change.
- **Title** links to the ADR file.
- The index and the ADR file must never be out of sync on **Status**, **Title**, or **Domain**.
- ADRs are **not** removed from this index; use **Deprecated** or **Superseded** (with a clear successor link) instead.

## Creating a new ADR

1. Copy [TEMPLATE.md](TEMPLATE.md).
2. Assign the next sequential ID: increment the numeric segment from the highest existing `ADR-*` file in `adrs/` (for example after `ADR-002-…` use `003` → `ADR-003-…`).
3. Name the file `ADR-NNN-short-kebab-title.md` under `adrs/` at the repository root.
4. Set **Status** to **Proposed**.
5. Fill required sections; remove instructional placeholder lines before review.
6. Add a row to the correct domain table below with status **Proposed**.
7. After human approval, set **Accepted** in the file and index.

---

## Index

### Platform

| ID | Domain | Title | Status | Description |
| --- | --- | --- | --- | --- |

### Architecture

| ID | Domain | Title | Status | Description |
| --- | --- | --- | --- | --- |

### Security

| ID | Domain | Title | Status | Description |
| --- | --- | --- | --- | --- |

### Quality

| ID | Domain | Title | Status | Description |
| --- | --- | --- | --- | --- |

### UI

| ID | Domain | Title | Status | Description |
| --- | --- | --- | --- | --- |

---

_Remove domain sections that do not apply. Add new `###` headings and tables when the project needs a stable category (for example **Mobile** or **Compliance**). Keep table columns consistent: **ID**, **Domain**, **Title**, **Status**, **Description**._
