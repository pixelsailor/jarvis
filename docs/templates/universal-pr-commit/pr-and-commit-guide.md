# PR and commit guide — REPLACE_WITH_PROJECT_NAME

How humans and agents describe changes in **pull request** bodies and **commit messages**. **Merge-ready policy** (gates, orchestration artifacts, tooling thresholds) belongs in a separate validation or workflow doc when the project defines one — for example `docs/validation-checklist.md`. This guide defines **what to write** so reviewers can classify intent, scope, and test evidence without opening every file.

**GitHub:** When the project uses GitHub, [`.github/pull_request_template.md`](../../.github/pull_request_template.md) pre-fills the same sections for new PRs.

After initialization, this repository must **not** depend on Jarvis or any external scaffolding repository to interpret this guide.

---

## 1. When this guide applies

| Change | PR body | Commit message |
| --- | --- | --- |
| Any PR to the default branch | Full template (summary buckets + test evidence) | N/A |
| Direct commits (no PR) | Optional short note in commit body | **Imperative subject** required; body optional |
| **Orchestrated pipeline commit** | N/A unless the human also opens a PR | **Required** when the Orchestrator runs `git commit` after Gate 6 — see [§1.1 Orchestrated pipeline commits](#11-orchestrated-pipeline-commits) |
| **Orchestrated run, no pipeline commit** | When opening a PR later, use PR sections below | Human or another agent commits outside the pipeline — guide still applies if they choose |
| **Trivial** (typo, comment-only, isolated non-architectural fix) | One-line summary; test section may be **N/A — trivial** | Imperative subject only |

### 1.1 Orchestrated pipeline commits

Not every orchestration run ends in a git commit. The **Orchestrator** commits only when:

1. The human **requests** a pipeline commit after Gate 6 (or equivalent), **and**
2. The human **confirms** in the same thread, **and**
3. `task-manifest.json` `flags` includes `orchestrated_commit_requested` before `git commit`.

When those conditions are met, the Orchestrator **must** follow [§5 Commit messages](#5-commit-messages) and, in the commit body when useful, cite paths to `build-log.md`, `test-report.md`, and `validation-report.md` under `.cursor/orchestrations/{task-id}/`. Do **not** load this guide on every agent session — it applies at commit time, not as an always-on Cursor rule.

Architecture-touching work must still satisfy whatever merge-ready gates the project documents. This guide does not replace them.

---

## 2. PR summaries — three buckets

Separate **what changed for users** from **what changed for the system** and from **documented drift**. Use the headings below in PRs. **Do not** require these buckets in commit messages (see [§5 Commit messages](#5-commit-messages)).

### Product / UX

User-visible behavior: flows, copy, loading states, accessibility fixes users would notice. If nothing user-facing changed, write **None** (do not omit the heading).

### Architecture / ADR

Durable design: new or updated ADRs, layering, data ownership, auth, deployment, secrets, or major boundaries. Link `adrs/ADR-NNN-….md` or state **None**.

### Alignment gaps

Intentional deviation from an **Accepted** ADR recorded in [`docs/adr-alignment-gaps.md`](./adr-alignment-gaps.md) (or the path named in `adrs/GOVERNANCE.md`): new or updated **GAP-*** rows with severity and remediation. If implementation matches cited ADRs, write **None**. Do not hide drift in code comments only.

**Example (PR excerpt):**

```markdown
### Product / UX

Settings page shows a clear error when the API is unreachable; offline read path unchanged.

### Architecture / ADR

ADR-003 Secrets boundary — server-only env vars for API keys.

### Alignment gaps

None.
```

---

## 3. Test evidence (required every PR)

Every PR must include **either** evidence that behavior was exercised **or** an explicit **untested risk** statement.

| Field | Content |
| --- | --- |
| **Commands run** | Verified commands from README § Development or manifests — for example unit tests, typecheck, lint, named manual step |
| **Coverage notes** | What was automated vs manual; map to acceptance criteria or checklist rows when the project uses them |
| **Untested risk** | **Required** when nothing was verified — what could break and why verification was deferred |

When the project maintains `docs/validation-checklist.md`, align test and merge-ready sections with that doc.

---

## 4. Merge-ready (architecture-touching PRs only)

**Delete this section** for trivial or non-architectural PRs. Add it when the change touches architecture, security boundaries, persistence authority, deployment, secrets, or other **Accepted** ADR scope.

Use project-defined checklist IDs when they exist. Until then, use this generic checklist:

- [ ] ADR created, updated, or explicit follow-up task with timeline
- [ ] Deviations from **Accepted** ADRs recorded in `docs/adr-alignment-gaps.md` if not fixed in scope
- [ ] Verified check/lint/test commands pass for touched paths, or pre-existing failures documented
- [ ] Orchestrated run (if applicable): validation and test artifacts linked with an explicit pass / pass-with-notes verdict

State **Orchestration not required** when the change is trivial or non-architectural per project orchestration docs.

Do **not** claim merge-ready in the PR summary unless the project's gates are satisfied (human approval where the workflow requires it).

---

## 5. Commit messages

Use **imperative** subjects (~72 characters): what the commit does, not what you did.

```
Fix retry banner when API returns 503

Test: manual — offline read; REPLACE_WITH_VERIFY_COMMAND lint.
```

| Part | Guidance |
| --- | --- |
| **Subject** | Required; imperative mood; optional `Fix GAP-NNN:` prefix when the commit primarily addresses a gap row |
| **Body** | Optional; short context or test line when the commit is review-worthy **without** a PR — **not** the three PR buckets |
| **Scope** | Conventional prefixes (`feat:`, `fix:`, `docs:`) are optional; match recent repository style |

Agents must **not** create git commits unless the user explicitly asks. **Orchestrator:** commit only with `orchestrated_commit_requested` and in-thread confirmation; message format per §5 and §1.1. Other roles do not commit unless the user directs them in that session.

---

## 6. Agents creating pull requests

When using GitHub CLI:

- Run `gh pr create` with a body that follows [`.github/pull_request_template.md`](../../.github/pull_request_template.md) (HEREDOC for formatting).
- Pull applicable rows from `docs/validation-checklist.md` into review comments when the change touches boundaries that checklist covers.

---

## 7. Related artifacts

| Topic | Typical location |
| --- | --- |
| Accepted ADRs | [`adrs/INDEX.md`](../../adrs/INDEX.md) |
| ADR compliance before build work | [`.cursor/rules/adr-compliance.mdc`](../../.cursor/rules/adr-compliance.mdc) when present |
| Alignment gaps register | [`docs/adr-alignment-gaps.md`](./adr-alignment-gaps.md) |
| Validation checklist | `docs/validation-checklist.md` when scaffolded |
| README documentation map | Root `README.md` § Documentation |

---

*Replace `REPLACE_WITH_PROJECT_NAME` and `REPLACE_WITH_VERIFY_COMMAND` once when copying from Jarvis templates. Optional provenance: note that this guide was adapted during project setup; do not link to Jarvis as a runtime dependency.*
