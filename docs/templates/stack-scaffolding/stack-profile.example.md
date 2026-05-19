# Stack profile — REPLACE_WITH_PROJECT_NAME

**Purpose:** Canonical, agent-readable record of the target project's **confirmed** language, framework, and toolchain. The root README § Technology Stack summarizes this file; when they disagree after an edit, reconcile here first, then update the README.

**Jarvis:** Copy to `docs/stack/stack-profile.md` during initialization. Do not link to the Jarvis repository in this file.

**Last verified:** YYYY-MM-DD  
**Primary package path:** `.` *(monorepo: e.g. `apps/web`)*  
**Evidence:** *(list manifest paths used, e.g. `package.json`, `pnpm-lock.yaml`)*

---

## Confirmed capabilities

| Field | Value | Notes |
| --- | --- | --- |
| Primary language | *(e.g. TypeScript)* | |
| Secondary languages | *(optional)* | |
| Framework | *(e.g. SvelteKit)* | |
| Runtime | *(e.g. Node.js)* | Omit if same as language default |
| Package manager | *(e.g. pnpm)* | From lockfile when present |
| UI / styling | *(optional)* | |
| Primary datastore | *(optional)* | |
| Test runners | *(names only)* | Commands: `docs/stack/commands.md` / README § Development |
| Deployment hint | *(optional)* | |

---

## Detection history (optional)

Brief log for resumability — not a substitute for git history.

| Date | Change |
| --- | --- |
| YYYY-MM-DD | Initial detection + user confirmation |

---

## Exclusions

Stack components **present in repo but out of scope** for new agent guidance (e.g. legacy subfolder). Empty if none.

| Path or component | Reason |
| --- | --- |
| | |

---

## Related target docs

| Doc | When |
| --- | --- |
| `docs/stack/commands.md` | Tooling exists — verified install and quality commands (`JR-STACK-003`) |
| `docs/stack/source-documentation.md` | Production code exists — comment syntax per language |
| `docs/validation-checklist.md` | Medium/large init — stack-specific validation rows |
| `docs/stack/upstream-references.md` | After `PROJ-STACK-002` — official docs and MCP index ([`selection.md`](../../stack-scaffolding/selection.md)) |
| `docs/stack/framework-guide.md` | Optional — target-specific conventions not in upstream docs |
| `.cursor/rules/` | After `PROJ-STACK-002` — framework topic rules (globs) |

---

## Checklist (handoff)

- [ ] Values match lockfiles and manifests at **Primary package path**
- [ ] Root README § Technology Stack agrees with this file
- [ ] No required links to Jarvis
- [ ] `PROJ-STACK-000` (or successor ID) marked complete with evidence in `docs/roadmap/backlog.md`
