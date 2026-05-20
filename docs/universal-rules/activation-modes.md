# Rule activation modes (`JR-RULE-001`)

How target projects choose **alwaysApply**, **globs**, and **manual** for `.cursor/rules/*.mdc` files so agents load the narrowest useful policy without bloating every session.

**Platform tasks:** `JR-RULE-001`  
**Related:** [`.cursor/rules/index.md`](../templates/universal-rules/index.md) (catalog template), [`authoring.md`](./authoring.md) (`JR-RULE-002`), [`adr-and-doc-citation.md`](./adr-and-doc-citation.md) (`JR-RULE-003`), [`../stack-scaffolding/selection.md`](../stack-scaffolding/selection.md) (stack rules default to globs)

## Why three modes exist

| Goal | Mechanism |
| --- | --- |
| **Correctness** | Cross-cutting policy (routing, ADR gate) must load even when the user opens one arbitrary file |
| **Efficiency** | Stack, API, and test playbooks load only when matching paths are in context |
| **Discoverability** | Rare playbooks stay **manual** but remain listed in `index.md` |

Cursor applies frontmatter at load time. The **index** is the human/agent catalog; it does not replace frontmatter.

## Mode reference

| Mode | Frontmatter | Loaded when | Typical content size |
| --- | --- | --- | --- |
| **alwaysApply** | `alwaysApply: true` | Every agent session | Short — routing tables, “read X before build,” boundary gates that apply repo-wide |
| **globs** | `globs: <pattern>` (omit `alwaysApply` or set `false`) | Matching files are in context | Medium — framework idioms, folder-scoped data/auth rules |
| **manual** | Neither `alwaysApply` nor `globs` | User names the rule or agent is directed to it | Variable — MCP workflows, infrequent audits |

Do **not** set `alwaysApply: true` and broad `globs` on the same file unless the team explicitly wants duplicate loading (almost never).

## Decision tree (pick one mode)

```text
Does violation of this rule require reading unrelated files?
├─ No  → globs scoped to those files/directories
└─ Yes → Could an agent edit any path and break architecture?
    ├─ Yes → alwaysApply (only if body stays short; else split)
    └─ No  → globs on the smallest directory set that covers risk
```

**Examples**

| Concern | Default mode | Rationale |
| --- | --- | --- |
| Repo layout and “which doc to open” | alwaysApply | `project-routing.mdc` |
| Consult Accepted ADRs before build work | alwaysApply | `adr-compliance.mdc` |
| Root README editing scope | globs `README.md` | `readme-governance.mdc` |
| Framework syntax (Svelte, React, …) | globs on verified production roots | Stack selection |
| Dexie / DB layer only | globs on `src/lib/db/**` etc. | Boundary is path-local; **target-owned** — Jarvis does not scaffold by default |
| Progressive enhancement / offline-style product matrix | **Target-only** — ADR + globs or alwaysApply **only** when README § Boundaries and Accepted ADRs declare that model | Jarvis must not add offline-first or connectivity rules unless the project supports them |
| PR/commit communication | **doc** `docs/pr-and-commit-guide.md`; **not** alwaysApply | Apply guide when the **Orchestrator** runs `git commit` after `orchestrated_commit_requested` — see [`../orchestration/task-folder-model.md`](../orchestration/task-folder-model.md) § Git and commits |
| Documentation conventions (JSDoc, `@component`) | **doc** `docs/documentation-conventions.md` by default | Revisit `JR-RULE-004` when reference material is available; no default `.mdc` until then |
| Orchestration artifact ownership | globs `.cursor/orchestrations/**` | Touches orchestration tree only |
| Merge-ready **MG-*** gates | globs orchestration + agents + guide | `workflow-gates.mdc` |

## alwaysApply budget (platform default)

| Init path | Starter alwaysApply count | Notes |
| --- | --- | --- |
| **Small** | 0–2 | Optional `project-routing` only; skip `adr-compliance` until `adrs/` exists |
| **Medium** | 2 | `project-routing`, `adr-compliance` |
| **Large** | 2 + user-approved boundary rules | Each extra alwaysApply needs explicit approval |

**Hard pause (Jarvis):** Do not add a **fourth** `alwaysApply: true` rule without user approval. At **three**, Jarvis should warn about token and overlap risk and suggest globs or doc extraction.

**Soft guidance:** Target `index.md` § Activation modes should list every alwaysApply rule by name so agents can see session weight at a glance.

## Topic-specific (globs) conventions

| Topic | Glob practice |
| --- | --- |
| **Prefer directory roots** | `src/routes/**`, `src/lib/api/**` over `**/*` |
| **One glob set per file** | Split rules when glob sets differ (framework vs API vs tests) |
| **Verify paths** | Patterns must match paths confirmed in stack-profile or README — no invented folders |
| **Tests** | Separate globs for `**/*.spec.ts`, `e2e/**`, etc., when conventions differ from production |
| **Orchestration** | `.cursor/orchestrations/**` plus optional `docs/validation-checklist.md` for checklist rules |

Register globs in the index **one-liner** (e.g. “globs `src/**/*.ts`”) so agents do not open the file to learn scope.

## manual rules

Use when:

- The workflow runs rarely (MCP doc fetch order, one-off migration playbook).
- Loading on every session or on common globs would mislead (rule assumes user invoked a tool).

Still add an **index.md** row with activation **manual** and a one-line when-to-use.

## Promoting and demoting modes

| Trigger | Action |
| --- | --- |
| README § Boundaries adds repo-wide non-negotiable | Consider new **Accepted** ADR + alwaysApply or globs boundary rule — not long prose in `project-routing` |
| Rule body grows past ~40 lines of enforcement | Extract narrative to `docs/<topic>.md`; rule keeps links + short MUST/SHOULD |
| alwaysApply count > 3 | Demote the least global rule to globs or doc; update index same session |
| ADR **Superseded** | Update or remove rules that cited it; adjust activation if scope shrank |
| Stack path removed from repo | Remove or narrow globs; delete index row |

## Relationship to agents

Agent contracts (`.cursor/agents/*.md`) are **not** `.mdc` rules. Do not set `alwaysApply` on agent markdown. Minimum read sets live in agent INDEX — see [`../universal-agents/minimum-read-sets.md`](../universal-agents/minimum-read-sets.md).

## Human input (pause points)

Jarvis must **stop and ask** before:

| Situation | Why |
| --- | --- |
| Fourth+ `alwaysApply` rule | Token and policy conflict risk |
| Converting doc-only scaffold (documentation conventions, PR guide) to alwaysApply | Product choice; universal default is doc-first |
| `globs: **/*` or repo-root `**` for stack playbooks | Loads near-always; defeats globs purpose |
| Dual primary frameworks at same package root | Which framework gets globs rules — see stack selection |
| Replacing existing team `.cursor/rules/` activation scheme | May encode CI or review policy |

Routine: one framework globs rule, starter two alwaysApply files, index rows — no extra approval.

## Decisions recorded for `JR-RULE-001`

| Topic | Default |
| --- | --- |
| Universal starters (alwaysApply) | `project-routing.mdc`, `adr-compliance.mdc` when `adrs/` exists or is imminent |
| Documentation conventions | Doc-only unless user promotes |
| PR/commit guide | Doc-only; Orchestrator applies on pipeline commit when `orchestrated_commit_requested` |
| Progressive enhancement / offline rules | **Never** Jarvis-default; target README + ADRs only |
| Documentation conventions rule | Doc-only until `JR-RULE-004` |
| Stack/framework rules | **globs** on verified production paths |
| Boundary rules tied to subtrees | **globs** unless README/ADR states repo-wide authority |
| Max alwaysApply without approval | **3** (warn at 3; block at 4+) |
| Index authority | Every `.mdc` has a row; activation column matches frontmatter |
