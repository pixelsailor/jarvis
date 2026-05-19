# Minimum intake questions (no target README)

When the target project has **no** root `README.md`, or the file is empty, a generic template only, or clearly stale (wrong product name, unrelated stack), Jarvis runs this intake **before** drafting.

**Output:** Answers inform a draft per [`outline.md`](./outline.md). Record non-obvious choices as `PROJ-README-*` notes or sub-bullets in `docs/roadmap/backlog.md`.

## Ask in one batch (default)

Present questions together to reduce round-trips. Skip a question only when the answer is already unambiguous from repo files **and** the user has not contradicted it.

### Required (always ask if not evidenced)

| # | Question | Why agents need it |
| --- | --- | --- |
| Q1 | **What is the project called**, and what is the **one-sentence** description? | Title and summary |
| Q2 | **Who is it for** (end users, internal team, library consumers, operators)? | Audience and tone |
| Q3 | **What problem does it solve** or what outcome does it enable? | Principles and capabilities |
| Q4 | **What are the non-negotiables?** (e.g. offline-first, no account required, AGPL only, single-tenant) | Principles and architecture boundaries |
| Q5 | **What is the intended stack** at a high level (language, framework, primary data store, deployment target)? | Technology stack section; greenfield or no manifests — see [`../stack-scaffolding/confirmation.md`](../stack-scaffolding/confirmation.md#greenfield-no-manifests) |
| Q6 | **What must never happen** architecturally? (e.g. secrets in client bundles, PII in logs, breaking public API without major version) | Architecture boundaries → ADR candidates |

### Required when signals are ambiguous

| # | Question | When to ask |
| --- | --- | --- |
| Q7 | **Initialization size:** small (README + minimal rules), medium (+ ADRs and doc conventions), or large (+ agents and orchestration)? | See [`scaffolding-map.md`](./scaffolding-map.md) |
| Q8 | **Is this greenfield or augmenting existing code?** If augmenting, which paths are authoritative? | Audit vs greenfield; avoid overwriting real stack |

### Ask only if relevant (do not block on silence)

| # | Question | When |
| --- | --- | --- |
| Q9 | **Optional enhancements:** auth, cloud sync, AI, payments, email — which are in scope vs explicitly out? | Principles, boundaries, `PROJ-STACK-*` |
| Q10 | **Data ownership:** where does durable user data live; what is cached or ephemeral? | Data ownership section |
| Q11 | **Existing docs to preserve** (paths)? | Audit merge; avoid duplicate ADRs |
| Q12 | **License / contribution** expectations? | Link only; do not invent legal text |

## Infer before asking (do not re-ask)

File-based stack inference uses [`../stack-scaffolding/detection.md`](../stack-scaffolding/detection.md) (confidence levels and conflicts). After README draft, confirm via [`../stack-scaffolding/confirmation.md`](../stack-scaffolding/confirmation.md).

Jarvis may **infer** and state assumptions in the draft for the user to correct:

| Signal | May infer |
| --- | --- |
| `package.json` + `svelte` | Stack section lists Svelte; Development uses verified scripts |
| `pyproject.toml` | Python toolchain; no npm commands |
| `go.mod` | Go module path as project name candidate |
| Existing `adrs/` | Do not recreate; link in Documentation |
| Monorepo layout | Project structure; confirm which package is the "product" (Q8) |

**Rule:** Inference fills **draft bullets**, not **principles**. If Q4 would change product intent, ask Q4 even when files exist.

## Do not ask (wastes sessions)

- Package manager preference when lockfiles already exist — use the lockfile's tool.
- Framework tutorial preferences — select stack docs during `PROJ-STACK-*`, not intake.
- Branding, logos, badges — unless user requests.
- Every ADR topic upfront — spawn `PROJ-ADR-*` from boundaries after README draft.

## Minimum bar to start drafting

Jarvis may draft when **Q1–Q6** are answered or responsibly inferred (with assumptions listed), and **Q7–Q8** are resolved or defaulted to **medium** with user notification.

If the user refuses detail: draft a thin README, mark gaps in `PROJ-README-*` on the backlog, and do not invent principles or stack.

## Recording answers

Prefer durable storage in the target repo:

1. README sections (intent)
2. `docs/roadmap/backlog.md` — `PROJ-README-001` with sub-bullets for open assumptions
3. First ADR only when a boundary needs Accepted status before implementation

Do not rely on Jarvis chat history or `.cursor/plans/` as the system of record.
