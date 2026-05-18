# Handoff self-containment — REPLACE_WITH_PROJECT_NAME

Verify the repository does **not** depend on the Jarvis platform repository, sibling workspace paths, or chat-only setup context for normal development. Run before completing `PROJ-HANDOFF-001` and `PROJ-HANDOFF-002` and before claiming initialization handoff.

**Related:** Root README checks (project README governance or intake checklist). Backlog gates: `docs/roadmap/backlog.md` → `PROJ-HANDOFF-*`. Initialization completion criteria: follow the project's roadmap handoff doc when vendored, or internal team process after setup.

After initialization, this repository must **not** require access to Jarvis or any external scaffolding repository to build, test, or extend the project.

---

## When to run

| Trigger | Action |
| --- | --- |
| Before `PROJ-HANDOFF-001` / `PROJ-HANDOFF-002` | Full pass; record evidence on backlog tasks |
| After copying any Jarvis universal template | Re-run **Links and paths** + **Rules and agents** sections |
| Before first external contributor or CI onboarding | Quick pass on **Documentation map** + **Tooling** |
| User reports "agent can't find docs" | Check for broken target-local links first — not missing Jarvis |

---

## Independence checks

Mark each item **pass** / **fail** / **N/A** when auditing. **Fail** on any required row blocks `PROJ-HANDOFF-*` until fixed or user approves partial handoff.

### Links and URLs

- [ ] **IND-01:** No `http`/`https` links to the Jarvis repository (or other scaffolding source repo) in generated docs under `docs/`, `adrs/`, `.cursor/`, `.github/`, or root `README.md` **as required reading**.
- [ ] **IND-02:** No `JR-*` platform task IDs presented as required reading for target contributors (backlog may mention historical setup in prose — not as live dependency).
- [ ] **IND-03:** Provenance, if present, states Jarvis (or other initializer) as **historical setup help** only — in `docs/roadmap/`, `CONTRIBUTING.md`, or similar — not as a runtime dependency in root README § Documentation.

### Paths and workspace layout

- [ ] **IND-04:** No relative paths that escape the target repo to reach Jarvis (for example `../jarvis/`, `../../Projects/jarvis/`).
- [ ] **IND-05:** Rules, agents, and docs use **target-root-relative** paths (`docs/…`, `adrs/…`, `.cursor/rules/…`) — not paths that only work when Jarvis sits beside the target in a multi-root workspace.
- [ ] **IND-06:** No `extends`, `import`, or `include` in config files pointing at files outside the target repository (unless explicitly documented third-party tools).

### Rules and agents

- [ ] **IND-07:** Every `see` / markdown link in `.cursor/rules/*.mdc` resolves inside the target repo.
- [ ] **IND-08:** Agent contracts under `.cursor/agents/` or `agents/` cite target paths only; minimum read sets do not list Jarvis-only files.
- [ ] **IND-09:** `.cursor/rules/index.md` does not require loading Jarvis templates or external rule packs at session start.

### Documentation map

- [ ] **IND-10:** Every path linked from root README § Documentation exists **or** has an open `PROJ-*` task with a stable ID.
- [ ] **IND-11:** Copied universal scaffolds (ADR index, conventions, validation checklist, PR guide) are **edited in place** — not symlinked to another repository.
- [ ] **IND-12:** No "copy from Jarvis `docs/templates/…`" instructions left as the **only** way to obtain a required doc (target must contain the file or a tracked task).

### Tooling and packages

- [ ] **IND-13:** Package manifests do not list Jarvis (or uninitialized scaffold packages) as dependencies for build, test, or runtime.
- [ ] **IND-14:** Scripts in README § Development and in rules match **verified** commands from this repo's manifests — not placeholder commands from templates.
- [ ] **IND-15:** CI/config does not clone or mount a sibling Jarvis repo for normal pipelines.

### Chat and backlog honesty

- [ ] **IND-16:** No setup promise exists only in chat — open work is recorded in `docs/roadmap/backlog.md` (or **Deferred** / **Cancelled** with reason).
- [ ] **IND-17:** README, ADRs, and backlog do not contradict on boundaries, stack, or data ownership (run README sync workflow if README changed late).

---

## Suggested scans (agents)

Run from the target repository root. Adjust patterns if the project uses different scaffolding names.

```bash
# Jarvis repo name in links (customize REMOTE_NAME if your scaffold used another label)
rg -i 'jarvis' --glob '!node_modules' --glob '!.git' docs adrs .cursor .github README.md 2>/dev/null || true

# Sibling escape paths (examples)
rg '\.\./.*jarvis|Projects/jarvis' --glob '!node_modules' --glob '!.git' 2>/dev/null || true

# Platform task IDs in generated docs (optional — flag JR- in non-roadmap docs)
rg 'JR-[A-Z]+-[0-9]+' docs adrs .cursor .github README.md 2>/dev/null || true
```

Review hits manually: **historical mentions** in `docs/roadmap/` may be acceptable; **required links** to Jarvis are not.

---

## Evidence for backlog tasks

When marking handoff tasks complete, add sub-bullets like:

```markdown
- [x] `PROJ-HANDOFF-001`: Confirm no generated doc links to the Jarvis repository. **required for handoff**
  - Evidence: `handoff-self-containment.md` IND-01–IND-06 pass 2026-05-18; `rg -i jarvis` clean outside docs/roadmap provenance note.

- [x] `PROJ-HANDOFF-002`: Confirm rules and agents cite only target-project paths. **required for handoff**
  - Evidence: IND-07–IND-09 pass; sampled `.cursor/rules/adr-compliance.mdc`, `docs/validation-checklist.md`.
```

`PROJ-HANDOFF-003` remains **human** acknowledgment — quote or paraphrase user consent with date.

---

## Failures → actions

| Failure | Action |
| --- | --- |
| Jarvis URL in rule or ADR | Replace with target-local path; re-run IND-01, IND-07 |
| `../jarvis/docs/…` in link | Copy content into target `docs/`; update link |
| Symlink to external template | Replace with committed file; `PROJ-DOC-*` if large |
| Rule cites missing target file | Create file or fix path; do not point at Jarvis |
| Invented script in README | Remove; `PROJ-STACK-*` with verified commands |
| Required work only in chat | Add `PROJ-*` rows; do not claim handoff |

---

## After handoff

| Item | Guidance |
| --- | --- |
| Re-run this checklist | When adding orchestration, new agent contracts, or bulk template imports |
| Root README link | Team may remove `docs/handoff-self-containment.md` from Documentation map when setup is complete |
| Ongoing independence | New ADRs and rules must keep target-local `see` links — same discipline as IND-07 |
| Jarvis for later questions | Optional for humans; **not** required for agents working in this repo |

---

## Checklist versioning

| Version | Date | Notes |
| --- | --- | --- |
| 1.0.0 | _YYYY-MM-DD_ | Initial checklist from Jarvis universal handoff template |

Bump when required rows change; sync `PROJ-HANDOFF-*` evidence expectations if IDs change.
