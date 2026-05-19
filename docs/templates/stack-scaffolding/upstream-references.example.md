# Upstream references — REPLACE_WITH_PROJECT_NAME

**Purpose:** Canonical **external** documentation and agent tooling for this stack. Agents read this before deep framework work. Do not link to the Jarvis repository.

**Derived from:** `docs/stack/stack-profile.md` (confirmed capabilities).  
**Last updated:** YYYY-MM-DD

---

## Framework

| Resource | URL | When to use |
| --- | --- | --- |
| *(e.g. Svelte)* | `https://svelte.dev/docs/svelte/llms.txt` | Component syntax, runes, reactivity |
| *(e.g. SvelteKit)* | `https://svelte.dev/docs/kit/llms.txt` | Routes, load, adapters |

---

## Libraries (optional)

| Library | URL | Notes |
| --- | --- | --- |
| | | |

---

## MCP or IDE doc tools (optional)

Only document tools **configured for this project**. Omit this section if none.

| Tool / server | Order or rule | Notes |
| --- | --- | --- |
| *(e.g. Svelte MCP)* | 1. `list-sections` → 2. `get-documentation` → 3. `svelte-autofixer` | Run autofixer on every edited `.svelte` file |

---

## Not in scope

List stack components present in the repo but excluded from agent guidance (mirror `stack-profile.md` § Exclusions).

| Component | Reason |
| --- | --- |
| | |

---

## Checklist

- [ ] URLs match confirmed framework/version in `stack-profile.md`
- [ ] No Jarvis or reference-repo links
- [ ] Linked from root README § Documentation
- [ ] `.cursor/rules/index.md` points here for stack doc workflow (if applicable)
