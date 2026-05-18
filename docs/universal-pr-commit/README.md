# Universal PR and commit communication scaffold

Generic guidance for how target projects describe changes in **pull requests** and **commit messages**. Jarvis copies and adapts these files so reviewers and future agents can classify work without reading every diff.

**Platform task:** `JR-UNIVERSAL-005`  
**Copy source:** [`../templates/universal-pr-commit/`](../templates/universal-pr-commit/)  
**Target layout:**

| Path | Role |
| --- | --- |
| `docs/pr-and-commit-guide.md` | Canonical policy (from `pr-and-commit-guide.md` template) |
| `.github/pull_request_template.md` | GitHub PR body pre-fill (from `pull_request_template.md` template) |

**No Cursor rule** for this scaffold (confirmed): agents load policy via the root README Documentation map and `.cursor/rules/index.md` (Workflow row). Projects may add a `PROJ-RULE-*` topic rule later if enforcement in `.cursor/rules/` is desired ([`universal-rules`](../universal-rules/README.md)).

## When to scaffold

| Initialization path | PR/commit guidance |
| --- | --- |
| **Small** | Optional — copy `docs/pr-and-commit-guide.md` when the repo will use PRs or multiple agents commit |
| **Medium** | Default — guide + `.github/pull_request_template.md` when using GitHub |
| **Large** | Required — same as medium; link both from root README § Documentation |

See [`../target-readme/scaffolding-map.md`](../target-readme/scaffolding-map.md).

## Copy and adapt (agents)

1. If the target already has `docs/pr-and-commit-guide.md` or `.github/pull_request_template.md`, **audit** — merge missing sections; do not replace team-specific policy without user approval.
2. Copy [`pr-and-commit-guide.md`](../templates/universal-pr-commit/pr-and-commit-guide.md) → `docs/pr-and-commit-guide.md`.
3. Replace `REPLACE_WITH_PROJECT_NAME` and `REPLACE_WITH_VERIFY_COMMAND` with verified values from README § Development and manifests.
4. When the target uses **GitHub**, copy [`pull_request_template.md`](../templates/universal-pr-commit/pull_request_template.md) → `.github/pull_request_template.md` (create `.github/` if missing).
5. If the target does not use GitHub, skip step 4; note in README § Documentation that PR bodies follow the guide manually.
6. Add one line to root README § Documentation for each path above.
7. Add a **Workflow** row to `.cursor/rules/index.md` pointing at `docs/pr-and-commit-guide.md` (doc-only — no `.mdc`).
8. Add or update `PROJ-DOC-*` rows in `docs/roadmap/backlog.md` with evidence paths.

**Handoff rule:** Generated files must not link to the Jarvis repository. All `see` targets must be target-project paths.

## Relationship to other Jarvis scaffolds

| Scaffold | Relationship |
| --- | --- |
| [`../universal-adr/README.md`](../universal-adr/README.md) | Architecture bucket links **Accepted** ADRs; alignment gaps use `docs/adr-alignment-gaps.md` |
| [`../universal-readme/README.md`](../universal-readme/README.md) | README governance routes PR/commit docs out of the root README |
| [`../universal-rules/README.md`](../universal-rules/README.md) | Index lists guide under Workflow; no default `pr-commit-expectations.mdc` |
| `JR-UNIVERSAL-006` (future) | `docs/validation-checklist.md` — merge-ready section in guide and PR template reference it when present |
| `JR-ORCH-*` (future) | Orchestrated runs link validation/test artifacts; guide §1 and §4 stay generic until orchestration docs exist |

## Human input (pause points)

Jarvis must **stop and ask** the user before:

- Replacing an existing `docs/pr-and-commit-guide.md` or PR template that encodes team-specific policy (for example monorepo-wide commit conventions or required ticket IDs in subjects).
- Requiring the three PR buckets in **commit** bodies when the team standardized on **subject-only** commits (platform default).
- Adding an **always-apply** Cursor rule for PR/commit when the project chose **doc-only** loading.
- Shipping a GitHub PR template when the project uses GitLab, Gerrit, or another host without an equivalent path.

Routine copy, placeholder replacement, and Documentation map links do not require extra approval.

## Decisions recorded for `JR-UNIVERSAL-005`

Defaults favor long-term agent efficiency; override per target when the user directs:

| Topic | Default |
| --- | --- |
| Canonical policy path | `docs/pr-and-commit-guide.md` |
| Cursor rule | **None** (doc-only; index row under Workflow) |
| GitHub PR template | **Yes** on medium/large when using GitHub — [`.github/pull_request_template.md`](../templates/universal-pr-commit/pull_request_template.md) |
| PR summary shape | Three buckets: **Product / UX**, **Architecture / ADR**, **Alignment gaps** |
| Commit messages | **Subject-only** default; imperative ~72 chars; optional body for context/test — **not** the three PR buckets |
| Test evidence | Required every PR; **Untested risk** required when nothing was verified |
| Merge-ready block | **Generic optional** section in guide + PR template; no universal `workflow-gates.mdc` |
| Validation checklist | Referenced when `docs/validation-checklist.md` exists (`JR-UNIVERSAL-006`); no hard dependency until scaffolded |
| WFD pattern source | Bucket discipline and test-evidence expectations generalized — no WFD product or stack identifiers in templates |
