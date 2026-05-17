# Canonical target README outline

Defines the sections Jarvis should generate or preserve in a target project's root `README.md`. Goal: one **10,000-foot** document that orients humans and agents without duplicating ADRs, rules, or playbooks.

## Design rules

1. **README = intent and routing.** Durable decisions belong in `adrs/`; enforced behavior in `.cursor/rules/` (or equivalent); procedures in topic docs under `docs/`.
2. **No Jarvis dependency.** Do not link to the Jarvis repository or require Jarvis-side paths in the target README after handoff.
3. **No invented tooling.** Commands and package managers appear only after verification from project files (see [`intake-questions.md`](./intake-questions.md) and [`handoff-checklist.md`](./handoff-checklist.md)).
4. **Stable headings.** Use the section titles below so agents can scan predictably across projects.
5. **Length budget.** Prefer roughly 120–250 lines for an initialized app or service; libraries and CLIs may be shorter. If longer, move detail behind links in **Documentation**.

## Section tiers

| Tier | Sections | When |
| --- | --- | --- |
| **Required** | 1–7 | Every initialized target project |
| **Conditional** | 8–11 | Include when README signals or user answers apply (see column notes) |
| **Defer** | — | Framework playbooks, API reference, test matrices, agent orchestration mechanics |

### Human default (documented; revisit in open decisions)

Until [`open-decisions.md`](../roadmap/open-decisions.md) closes mandatory scaffold policy, Jarvis uses **one canonical outline** for all project sizes. **Small** projects omit optional sections and shorten bullets; they do not use a different heading order.

---

## Required sections

### 1. Title and summary

- **H1:** Project name (matches repo or product name).
- **First paragraph (2–4 sentences):** What it is, who it is for, and the core outcome. No stack dump here.

### 2. Principles (or non-negotiables)

- **Heading:** `## Principles` or `## Product Principles` (product) / `## Design Principles` (library, platform, infra).
- **Content:** 5–10 bullet **statements** of durable intent (offline-first, API stability, multi-tenant isolation, etc.).
- **Not here:** Implementation steps, library versions, or feature backlogs.

### 3. Core capabilities

- **Heading:** `## Core Capabilities`
- **Content:** Major user- or integrator-visible capabilities as a short bullet list.
- **Scope:** What the system *does*, not how folders are laid out.

### 4. Technology stack

- **Heading:** `## Technology Stack`
- **Content:** High-level stack only (language, framework, primary datastore, hosting model, test runners **by name**).
- **Not here:** Version pins unless they are architectural constraints; config file paths; long install guides.

### 5. Architecture boundaries

- **Heading:** `## Architecture Boundaries`
- **Content:** Client vs server, sync vs async ownership, secret boundaries, extension points, schema-led vs ad hoc data, multi-tenant boundaries — **as statements**.
- **Route detail to:** First Accepted ADRs on those topics (linked once `adrs/INDEX.md` exists).

### 6. Documentation

- **Heading:** `## Documentation` or `## Documentation Map`
- **Content:** Table or bullet list of **target-project** links only.
- **During setup, always include:** `docs/roadmap/README.md` (setup backlog).
- **Add as created:** `adrs/INDEX.md`, `.cursor/rules/index.md`, orchestration guide, PR/commit guide, topic architecture docs.
- **Not here:** External framework tutorials; duplicate rule text.

### 7. Development (minimal onboarding)

- **Heading:** `## Development`
- **Content:** Verified install, dev, and quality commands only (from `package.json`, `Makefile`, `pyproject.toml`, etc.).
- **Env vars:** Names and **which runtime** loads them (client, server, CI) — not secret values.
- **Not here:** Deployment runbooks, architecture essays, or commands Jarvis has not verified.

---

## Conditional sections

Include when applicable; omit entirely if not relevant (do not leave empty placeholders).

### 8. Data ownership

- **When:** Persistent user data, caching, sync, PII, or multi-store boundaries matter.
- **Heading:** `## Data Ownership`
- **Content:** What is authoritative where (device, server, object store); what is ephemeral; optional cloud/account enhancement in one short paragraph.

### 9. Roadmap direction

- **When:** Product with planned themes (not required for one-off libs or internal tools).
- **Heading:** `## Roadmap Direction`
- **Content:** Near-term themes only; link to issue tracker or product doc if it exists.
- **Not here:** Checkbox task lists (those live in `docs/roadmap/backlog.md`).

### 10. Project structure

- **When:** Repo layout is non-obvious or agents need a compass.
- **Heading:** `## Project Structure`
- **Content:** Compact tree (10–20 lines) or "see `docs/architecture.md`" once that doc exists.
- **Not here:** Per-file narratives.

### 11. Contributing and license (existing files only)

- **When:** `CONTRIBUTING.md` or `LICENSE` already exists — link from Documentation or a one-line footer.
- **Do not** generate legal or contribution policy text without human-provided content.

---

## Anti-patterns (do not put in target README)

| Anti-pattern | Put instead |
| --- | --- |
| Full Cursor rule bodies | `.cursor/rules/` + link in Documentation |
| ADR bodies | `adrs/ADR-NNN-*.md` + index link |
| Jarvis or WFD references required for operation | Optional one-line provenance in `docs/roadmap/` only |
| Unverified `pnpm` / `npm` / `docker` commands | Verify files first; or omit Development until verified |
| Chat-only decisions | Accepted ADR or roadmap task |
| "TODO: fill in stack" without a `PROJ-*` task | `docs/roadmap/backlog.md` |

---

## Skeleton

Copy into the target project and replace bracketed placeholders.

```markdown
# [Project Name]

[What it is, who it is for, and the primary outcome in 2–4 sentences.]

## Principles

- **[Principle name]:** [One sentence.]
- ...

## Core Capabilities

- ...

## Technology Stack

- **Application:** [language, framework]
- **Data:** [primary stores]
- **Hosting / runtime:** [e.g. serverless, container, edge]
- **Quality:** [test/lint tools by name]

## Architecture Boundaries

[Client/server, secrets, schema ownership, extension points — short paragraphs or bullets.]

## Data Ownership

*(Include only when durable data boundaries matter.)*

## Roadmap Direction

*(Include only for product roadmaps.)*

## Project Structure

```text
[compact tree]
```

## Documentation

| Topic | Location |
| --- | --- |
| Setup backlog | [`docs/roadmap/README.md`](./docs/roadmap/README.md) |
| Architecture decisions | [`adrs/INDEX.md`](./adrs/INDEX.md) *(when created)* |
| Agent rules | [`.cursor/rules/`](./.cursor/rules/) *(when created)* |

## Development

**Prerequisites:** [verified]

```bash
[verified install command]
[verified dev command]
[verified check/lint/test commands]
```

**Environment:** `[VAR_NAME]` — [client | server | CI]; do not commit secrets.
```
