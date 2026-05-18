# README / ADR alignment gaps — REPLACE_WITH_PROJECT_NAME

Tracks **known** mismatches between **Accepted** ADRs (and README boundaries) and current implementation. This is a **debt register**, not a substitute for ADRs.

**When to use:** Implementation cannot match an **Accepted** ADR in the same change, and the team chooses to ship with documented drift. Remove or close rows when remediated.

**When not to use:** Decisions still under debate (**Proposed** ADRs), or one-off bugs with no architectural implication.

## Agent instructions

- Before architectural work, read [adrs/INDEX.md](../adrs/INDEX.md) for **Accepted** ADRs **and** scan open rows here.
- Do not mark a gap **closed** without evidence (PR link, commit, or test note).
- New gaps need a human-visible owner when the team uses ownership; otherwise use **Unassigned**.

## Field conventions

| Field | Required | Meaning |
| --- | --- | --- |
| **ID** | Yes | Stable id: `GAP-001`, `GAP-002`, … never reused |
| **Severity** | Yes | `blocker` · `high` · `medium` · `low` |
| **ADR(s)** | Yes | Accepted ADR id(s) violated or partially met |
| **Affected** | Yes | Paths, modules, or behaviors |
| **Summary** | Yes | One sentence of the drift |
| **Remediation** | Yes | Intended fix or ADR supersession plan |
| **Owner** | No | Person or team |
| **Blocks** | No | Future work that must not proceed until fixed |

## Open gaps

| ID | Severity | ADR(s) | Affected | Summary | Remediation | Owner | Blocks |
| --- | --- | --- | --- | --- | --- | --- | --- |
| *(none yet)* | | | | | | | |

## Closed gaps

| ID | Closed | Evidence |
| --- | --- | --- |
| *(none yet)* | | |

---

_Copy to `docs/adr-alignment-gaps.md` in the target project when the first gap is recorded. Link from README Documentation or [GOVERNANCE.md](../adrs/GOVERNANCE.md) §8.3._
