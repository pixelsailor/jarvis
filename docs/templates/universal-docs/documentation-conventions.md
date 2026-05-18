# Documentation conventions — REPLACE_WITH_PROJECT_NAME

Authoritative guidance for **how** this repository documents production source, tests, architecture writeups, and user-facing guides. Humans and agents should treat this file as the contract; stack-specific syntax (JSDoc, docstrings, component blocks) belongs in linked stack docs, not duplicated here.

**Scope:** All documentation except the root README (see [`readme-governance.md`](./readme-governance.md)) and durable decisions in `adrs/`. Setup planning lives in `docs/roadmap/` and is not covered here.

After initialization, this repository must **not** depend on Jarvis or any external scaffolding repository to interpret these conventions.

---

## 1. Goals

- **Intent visible in-editor:** Production code documents *why* and *contracts*, not repeated types.
- **One home per concern:** ADRs own decisions; rules enforce behavior; this doc owns *documentation shape*; topic docs own depth.
- **Agent-efficient routing:** Predictable paths (`docs/architecture/`, `docs/guides/`) so cold sessions need minimal search.
- **Honest exemptions:** Tests, generated code, and thin barrels are documented lightly by design.

---

## 2. Production source

### 2.1 What to document

| Surface | Expectation |
| --- | --- |
| **Substantive modules** | File-level summary: what the module does and who consumes it |
| **Exported functions, classes, types** | Summary; parameters and return *meaning*; notable failure modes |
| **Non-obvious constants and config** | One-line or `@remarks`-style note when the name is insufficient |
| **Behavioral contracts** | Side effects (I/O, persistence), caching, idempotency, concurrency, streaming — when not obvious from types |

**Substantive** means the file exports symbols used outside the file or encodes non-trivial behavior. Trivial re-exports, generated output, and env-only schema files may use a single file-level line or rely on stack exemptions.

### 2.2 Universal rules (all stacks)

- Describe **meaning and constraints**, not syntax types already expressed in code.
- Prefer present tense or imperative one-sentence summaries.
- Document **public and protected** API; skip noise on trivial getters unless behavior is surprising.
- Use **`@deprecated`** (or stack equivalent) with a short migration note when sunsetting.
- Cross-link important boundaries with **`@see`** / “see `path`” to **Accepted** ADRs or topic docs — do not paste ADR bodies into source comments.

### 2.3 Exemptions (default)

Unless the project’s stack extension doc says otherwise:

| Area | Convention |
| --- | --- |
| **Tests** | See [§4 Tests](#4-tests) — not production standard |
| **Scripts and tooling** | `scripts/`, one-off CLIs, codegen drivers — file-level summary when non-obvious |
| **Generated or vendored trees** | Do not hand-edit docs; exclude from lint/doc checks if applicable |
| **Pure re-export barrels** | Optional single file-level line |
| **Config-only files** | One-line file summary when symbols are self-explanatory |

### 2.4 Stack extensions (required when code exists)

Record language- and framework-specific rules in a **project-owned** doc linked from the root README Documentation map, for example:

- `docs/stack/source-documentation.md` — JSDoc, docstrings, `///` comments, Rust doc comments, Svelte `@component` blocks, OpenAPI generation, etc.

Jarvis or agents must **verify** the stack from manifests and README § Stack before filling that file. Do not invent syntax rules for a stack the repo does not use.

**Anti-patterns**

| Do not | Do instead |
| --- | --- |
| Duplicate types in doc comments | Describe behavior and units |
| Empty filler (“returns a value”) | Omit or write a concrete sentence |
| Paste ADR or rule text into every file | Link once at module or package boundary |
| Document every private helper | Document the exported contract |

### 2.5 Per-change checklist (production)

- [ ] New substantive modules have a file-level summary
- [ ] Exported API with non-obvious parameters or errors is documented
- [ ] Side effects and caching called out when types do not convey them
- [ ] Stack extension doc followed for comment syntax and component docs

---

## 3. Contributor and technical docs (`docs/`)

### 3.1 Routing table

| Kind | Default path | Purpose |
| --- | --- | --- |
| **This file** | `docs/documentation-conventions.md` | Doc shape for source, tests, architecture, guides |
| **README policy** | `docs/readme-governance.md` | Root README only |
| **Architecture** | `docs/architecture/` | System design, boundaries, flows — [§5](#5-architecture-documentation) |
| **User-facing guides** | `docs/guides/` | End-user or customer docs — [§6](#6-user-facing-documentation) |
| **Stack / tooling** | `docs/stack/` or `docs/<topic>.md` | Commands, lint, source comment syntax, framework playbooks |
| **Setup backlog** | `docs/roadmap/` | `PROJ-*` initialization tasks — not product documentation |
| **ADR drift** | `docs/adr-alignment-gaps.md` | Optional; implementation vs Accepted ADR |

Do not mix **setup** checklists (`docs/roadmap/backlog.md`) with **product** roadmaps described in the root README.

### 3.2 Topic doc shape

Each `docs/<topic>.md` (outside `guides/` and `architecture/`) should start with:

1. **One-line purpose** — what question this file answers.
2. **Audience** — contributor, operator, or integrator.
3. **See also** — links to ADRs, architecture topics, or rules (target-local paths only).

Use stable `##` headings so agents can scan. Prefer updating an existing topic file over spawning near-duplicates.

### 3.3 Anti-patterns

| Do not | Do instead |
| --- | --- |
| Duplicate Accepted ADR bodies | Link `adrs/ADR-NNN-*.md` |
| Duplicate Cursor rule bodies | Link `.cursor/rules/index.md` |
| Store secrets or env values | Names and runtime only (see readme-governance) |
| Link to Jarvis as required reading | Target-local paths only |

---

## 4. Tests

Tests document **behavior under test**, not production API surface.

| Element | Convention |
| --- | --- |
| **File or suite** | Short header comment or `describe` block: scope, invariants, and what is *out* of scope |
| **Cases** | Names state behavior (`rejects empty title`, not `test1`) |
| **Shared fixtures/helpers** | Document setup assumptions, mutability, and cleanup in the helper file or a colocated `README.md` |
| **Snapshots / golden files** | Note what change triggers intentional snapshot updates |

**Exemptions:** Individual test cases do not need per-case prose when the name is sufficient. Do not apply production JSDoc/docstring standards to every `it`/`test` block.

**Evidence:** When a `PROJ-*` or ADR task requires test coverage, point to spec paths or commands in the backlog row — not in this file.

---

## 5. Architecture documentation

**Home:** `docs/architecture/` with a short **`README.md` index** listing topic files.

### 5.1 What belongs here

- System context and major components
- Data flow and authority (who owns writes, cache vs source of truth)
- Trust boundaries (client, server, third parties)
- Deployment topology at the diagram level
- Cross-cutting patterns (events, jobs, feature flags) when not yet ADR material

### 5.2 What belongs in ADRs instead

Record **durable decisions** and alternatives rejected in `adrs/`. Architecture docs **explain and illustrate** current shape; they do not replace ADR status or governance.

When a topic doc and an Accepted ADR disagree, the ADR wins until the ADR is superseded or the gap is logged in `docs/adr-alignment-gaps.md`.

### 5.3 Topic file conventions

- Filename: `kebab-case.md` (for example `data-flow.md`, `deployment.md`).
- Start with purpose + audience; link relevant **Accepted** ADRs in **See also**.
- Use diagrams (Mermaid or ASCII) when they reduce ambiguity; keep them maintainable.
- Note **non-goals** when scope is often misunderstood.

### 5.4 Index maintenance

When adding `docs/architecture/<topic>.md`, update `docs/architecture/README.md` and add one line to the root README Documentation map.

---

## 6. User-facing documentation

**Home:** `docs/guides/` for instructions aimed at **end users, customers, or operators** who do not read the repository for development.

| Belongs in `docs/guides/` | Belongs elsewhere |
| --- | --- |
| How to use the product feature | API contracts → OpenAPI or `docs/api/` if used |
| Account, billing, or support flows | Implementation detail → `docs/architecture/` or ADRs |
| Troubleshooting for users | Contributor setup → root README § Development |
| Release notes for users | Version history for developers → `CHANGELOG.md` if maintained |

**Tone:** Plain language, imperative steps, expected outcomes. Avoid internal codenames and `PROJ-*` IDs.

**Linking:** Root README Documentation map lists the guides index (`docs/guides/README.md` or key guides). Do not paste long user guides into the root README.

---

## 7. Maintenance workflow

### 7.1 Material vs immaterial doc edits

**Material** (same session follow-up):

- New architectural topic or changed trust/data boundaries
- New user-facing workflow
- Changed documentation routing (paths in this file or README map)
- New production documentation standard affecting stack extension doc

**Immaterial:** Typos, formatting, clarifications that do not change obligations.

### 7.2 Sync with README and backlog

When the root README gains a Documentation map entry pointing at a new path:

- Create the file or open a `PROJ-DOC-*` task in `docs/roadmap/backlog.md`.
- Follow [`readme-governance.md`](./readme-governance.md) §6 for README ↔ backlog sync.

When **Accepted** ADRs change boundaries architecture docs describe, update `docs/architecture/` or open `PROJ-DOC-*` in the same session.

### 7.3 Agent read order (documentation work)

1. Root `README.md` (Documentation map)
2. This file
3. `docs/readme-governance.md` if touching root README
4. `adrs/INDEX.md` + relevant **Accepted** ADRs
5. `docs/architecture/README.md` or specific topic
6. `docs/stack/source-documentation.md` (or equivalent) before editing production comments
7. `docs/roadmap/backlog.md` while setup is active

---

## 8. Relationship to other artifacts

| Artifact | Relationship |
| --- | --- |
| `docs/readme-governance.md` | Owns root README; routes here for non-README doc policy |
| `adrs/` | Owns decisions; architecture docs illustrate |
| `.cursor/rules/` | Enforces behavior; does not duplicate this file (no required doc-conventions rule per project policy) |
| `docs/roadmap/` | Setup tasks only |
| Stack extension doc | Owns comment syntax and framework-specific doc blocks |

---

## 9. Review checklist (before claiming doc conventions done)

- [ ] `docs/architecture/README.md` exists or has an open `PROJ-DOC-*` task
- [ ] `docs/guides/README.md` exists when the product has external users, or section omitted with reason
- [ ] Stack source-documentation path exists or is explicitly N/A (doc-only library, etc.)
- [ ] Root README Documentation map links this file
- [ ] No Jarvis or scaffolding-repo URLs required for operation
- [ ] Architecture topics link Accepted ADRs instead of duplicating decision prose

---

*Replace `REPLACE_WITH_PROJECT_NAME` once when copying from Jarvis templates. Optional provenance: note that conventions were adapted during project setup; do not link to Jarvis as a runtime dependency.*
