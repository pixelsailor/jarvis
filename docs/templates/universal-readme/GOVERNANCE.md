# README governance — REPLACE_WITH_PROJECT_NAME

Authoritative guidance for the repository **root `README.md`**. Humans and agents should treat this file as the contract for what belongs in the README versus ADRs, rules, and topic docs.

**Scope:** Root `README.md` only. Directory-scoped `README.md` files follow the same *routing* idea but may include module-specific patterns; keep them short and link upward to this doc when unsure.

---

## 1. Purpose of the root README

The root README is the **10,000-foot control document**: what the project is, who it is for, durable principles, high-level stack, architecture boundaries, and **where to read next**. It orients cold sessions without chat history.

It is **not** an implementation manual, agent rule dump, ADR archive, or framework tutorial.

After project initialization, this repository must **not** depend on Jarvis or any external scaffolding repository to interpret the README.

---

## 2. Standard sections (heading contract)

Use stable headings so agents can scan across projects. Required sections apply to application and service repos unless noted.

| Section | Heading examples | Content |
| --- | --- | --- |
| **Summary** | `# [Project Name]` + first paragraph | What, who, primary outcome — no version dump |
| **Principles** | `## Principles`, `## Product Principles`, `## Design Principles` | 5–10 durable intent bullets; not implementation steps |
| **Capabilities** | `## Core Capabilities` | User- or integrator-visible outcomes |
| **Stack** | `## Technology Stack` | Language, framework, primary data store, hosting, quality tools **by name** |
| **Boundaries** | `## Architecture Boundaries` | Client/server, secrets, data authority, extension points — **statements**, not procedures |
| **Documentation** | `## Documentation`, `## Documentation Map` | Table or bullets of **in-repo** links only |
| **Development** | `## Development` | Verified install/dev/check commands; env var **names** and runtime (client/server/CI) |

**Conditional sections** (include when applicable; omit entirely if not relevant):

| Section | When |
| --- | --- |
| `## Data Ownership` | Persistent data, sync, PII, or multi-store boundaries matter |
| `## Roadmap Direction` | Product with near-term themes (not checkbox backlogs) |
| `## Project Structure` | Layout is non-obvious; compact tree or pointer to `docs/architecture.md` |

**Length:** Roughly 120–250 lines for a typical app or service; shorter for libraries and CLIs. If longer, move detail behind Documentation links.

---

## 3. Belongs in the root README

- Project purpose, audience, and non-negotiable principles
- High-level technology stack and where secrets or privileged operations run
- Short architecture boundary statements (no step-by-step runbooks)
- Compact project tree **or** a single pointer to a structure doc
- **Documentation map** with working links to ADRs, rules, setup backlog, and topic docs
- Minimal onboarding: prerequisites, verified commands, environment variable names (never values)

---

## 4. Put elsewhere (and link from the map)

| Instead of expanding the root README | Use |
| --- | --- |
| Durable architecture or ownership decisions | `adrs/` — record in an ADR, link `adrs/INDEX.md` |
| Enforced agent or contributor behavior | `.cursor/rules/` (or equivalent) + rules index |
| Feature flows, APIs, deep design | `docs/<topic>.md` or scoped `src/.../README.md` |
| Setup and foundation tasks | `docs/roadmap/backlog.md` (`PROJ-*` tasks) while initialization is active |
| Implementation vs Accepted ADR drift | `docs/adr-alignment-gaps.md` (optional; see `adrs/GOVERNANCE.md`) |
| PR/commit templates, validation checklists, orchestration mechanics | Dedicated docs linked once from the map |
| Production source, tests, architecture topics, user guides | `docs/documentation-conventions.md`; `docs/architecture/`; `docs/guides/` |

**Default action when adding information:** create or extend a linked doc (or ADR), then add **one line** to the Documentation map — do not paste long sections into the README.

---

## 5. Anti-patterns

| Do not | Do instead |
| --- | --- |
| Paste full Cursor rule bodies | Link `.cursor/rules/index.md` |
| Paste ADR bodies | Link `adrs/INDEX.md` and specific ADR files |
| Invent package commands | Verify from manifests; or omit Development until verified |
| Leave "TBD" sections with no task | Open `PROJ-*` in `docs/roadmap/backlog.md` or remove the section |
| Rely on chat-only decisions | Accepted ADR or roadmap task |
| Link to Jarvis or other scaffolding repos as required reading | Target-local paths only |
| Duplicate product roadmap checklists | `docs/roadmap/backlog.md` for setup; issue tracker for product work |

---

## 6. Maintenance workflow

### 6.1 Material vs immaterial edits

**Material** (update downstream artifacts in the **same session**):

- Principles, audience, scope, or non-negotiables
- Core capabilities (add/remove/rename)
- Technology stack components
- Architecture boundaries or data ownership
- Documentation map links (new, removed, retargeted)
- Development commands or environment variable names
- Statements that contradict open `PROJ-*` tasks or **Accepted** ADRs

**Immaterial** (README-only is fine):

- Typos, formatting, badges, contributor lists
- Wording that does not change behavior or artifact expectations

When unsure, treat as **material**.

### 6.2 README → backlog sync

If the project maintains a setup backlog at `docs/roadmap/backlog.md` (`PROJ-*` tasks):

- Apply §6.1 material vs immaterial classification.
- For each **material** change, add, reopen, or update matching `PROJ-{AREA}-*` rows in the **same session**.
- If the repo includes `docs/roadmap/readme-sync.md` (or another team-owned sync doc linked from `docs/roadmap/README.md`), follow it for section-to-prefix mapping and stale-artifact handling.

Every new README obligation needs a `PROJ-*` row or the README promise must be removed.

### 6.3 README ↔ ADR alignment

- README must not contradict **Accepted** ADRs.
- If implementation drifts from README intent or Accepted ADRs and cannot be fixed in the same change, record the gap in `docs/adr-alignment-gaps.md` (or the path named in `adrs/GOVERNANCE.md`).
- When an **Accepted** ADR changes boundaries the README summarizes, update the README in the same session or open a `PROJ-README-*` task.

### 6.4 Post-handoff

Owners may trim Documentation map entries (for example archive `docs/roadmap/` after setup), add product links, and refine principles without external scaffolding tools. Keep this governance file accurate when README policy changes.

---

## 7. Agent read order (cold session)

1. Root `README.md`
2. `docs/roadmap/README.md` and `docs/roadmap/backlog.md` (while setup is active)
3. This file (`docs/readme-governance.md`) when editing or auditing the README
4. `adrs/INDEX.md` and relevant **Accepted** ADRs for boundaries in scope
5. `.cursor/rules/index.md` for enforced behavior

---

## 8. Relationship to other docs

| Artifact | Relationship |
| --- | --- |
| `adrs/GOVERNANCE.md` | ADRs own durable decisions; README summarizes and routes |
| `.cursor/rules/` | Rules enforce behavior; README does not duplicate rule text. Include `readme-governance.mdc` (medium/large init) so README edits load this policy |
| `docs/roadmap/` | Resumable setup work; not a substitute for product roadmaps in README § Roadmap Direction |
| Scoped `README.md` under `src/` | Deeper local patterns; link from Documentation map when agents need them |

---

## 9. Review checklist (before claiming README done)

- [ ] Required sections present; conditional sections only when applicable
- [ ] No Jarvis or scaffolding-repo URLs required for operation
- [ ] Documentation map links exist or have open `PROJ-*` tasks
- [ ] Development commands verified from project manifests
- [ ] README aligns with **Accepted** ADRs (or gaps doc is updated)
- [ ] No playbook dump (rules, ADRs, orchestration) in the README body

---

*Replace `REPLACE_WITH_PROJECT_NAME` once when copying from Jarvis templates. Optional provenance: note that README governance was adapted during project setup; do not link to Jarvis as a runtime dependency.*
