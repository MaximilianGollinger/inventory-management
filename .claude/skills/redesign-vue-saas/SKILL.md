---
name: redesign-vue-saas
description: Redesign this Vue 3 client app into a modern SaaS UI — light sidebar layout (Vercel/Linear/Stripe style), consolidated design tokens, refined spacing and typography scales, polished cards and buttons, and lucide-vue-next icons. Invoke when the user says "redesign the UI", "modernize the app", "make it look like Linear/Vercel/Stripe", "convert top nav to sidebar", "clean up the design", or runs /redesign-vue-saas. Project-scoped to the inventory-management client at client/src. Orchestrates discover → propose → implement → verify, delegating every .vue create/edit to the vue-expert subagent per project mandate.
---

# /redesign-vue-saas — Modern SaaS visual refresh for the Vue 3 client

Full visual refresh: top nav → light left sidebar, consolidated design tokens, formalized spacing and typography scales, polished cards and buttons, lucide-vue-next icons. Project-scoped to `client/src`. **Orchestrator skill** — does not edit `.vue` files directly; delegates each create/edit to the `vue-expert` subagent.

## When to invoke

| Trigger | Example phrasing |
|---|---|
| Explicit slash | `/redesign-vue-saas` |
| UI refresh request | "redesign the UI", "modernize the app", "make it look like Linear/Vercel/Stripe" |
| Structural ask | "convert top nav to sidebar", "left navigation", "sidebar layout" |
| Polish ask | "clean up the design", "professional polish", "consistent spacing" |

## When NOT to invoke

- Micro CSS tweaks on a single component → edit directly or fire one vue-expert Task.
- Projects using **Tailwind, PrimeVue, Vuetify, Element Plus, Naive UI, or Quasar** — STOP and surface. This skill's CSS-variable approach conflicts.
- Vue 2 projects or non-Vue projects → STOP.
- Apps where the current top nav is the desired final state.

## Required references

**REQUIRED REFERENCE:** `.claude/skills/redesign-vue-saas/DESIGN.md` — the visual opinion. Read fully before proposing or delegating any edit.

**REQUIRED REFERENCE:** `.claude/skills/redesign-vue-saas/PLAYBOOK.md` — discovery probes and Playwright verification recipe.

## Hard constraints (project CLAUDE.md)

Non-negotiable. Carry to working memory each phase.

- **All `.vue` creates/edits delegate to `vue-expert`.** No exceptions. The skill writes `.css` / `.json` / `.js` directly; every `.vue` touch goes through a Task call.
- **Browser verification via `mcp__playwright__*`** against `http://localhost:3000`.
- **No emojis** in any UI string.
- **Preserve i18n** — `useI18n` composable, existing `nav.*` keys, `client/src/locales/en.js` and `ja.js`.
- **Preserve router paths** and **all modal mounts** in `App.vue`.
- **Options API + `setup()`** — project convention. Do NOT use `<script setup>`.

## Phase 1: Discover

Read these files and extract the listed facts. Do NOT propose or edit yet.

| File | Extract |
|---|---|
| `client/src/App.vue` | Top-nav element, `<router-link>` items, mounted modal components, FilterBar position |
| `client/src/main.js` | Vue version, full route list, existing CSS imports |
| `client/package.json` | `dependencies.vue` version, presence of conflicting frameworks |
| `client/src/views/` | View file list (for token migration pass) |
| `client/src/composables/useI18n.js` | `t()` signature, locale loading |
| `client/src/locales/en.js`, `ja.js` | Existing `nav.*` keys (note any missing) |

**Blockers — STOP and surface if any:**
- Tailwind / PrimeVue / Vuetify / Element Plus / Naive UI / Quasar in deps.
- `dependencies.vue` is `^2.x` or missing.
- `client/src/styles/tokens.css` already exists and differs from the template.
- `client/src/components/AppSidebar.vue` already exists.

Output a one-screen discovery summary to the user: detected routes, view count, blockers (if any), i18n key gaps (if any).

## Phase 2: Propose

Derive the sidebar item list from router-links found in `main.js`. Default mapping:

| Route | i18n key | Icon (lucide-vue-next) |
|---|---|---|
| `/` | `nav.overview` | `LayoutDashboard` |
| `/inventory` | `nav.inventory` | `Boxes` |
| `/orders` | `nav.orders` | `ShoppingCart` |
| `/spending` | `nav.finance` | `DollarSign` |
| `/demand` | `nav.demandForecast` | `TrendingUp` |
| `/reports` | `nav.reports` | `FileText` |

If the detected route set differs, build the item list from actual routes and pick icons by semantic match (escalate to user if uncertain).

Build a file-change table:

| File | Action | Who |
|---|---|---|
| `client/src/styles/tokens.css` | NEW | skill (direct) |
| `client/src/main.js` | EDIT (add import) | skill (direct) |
| `client/package.json` | EDIT (add `lucide-vue-next`) | skill (direct, user-gated install) |
| `client/src/locales/en.js`, `ja.js` | EDIT (add `nav.reports` if missing) | skill (direct) |
| `client/src/components/AppSidebar.vue` | NEW | **vue-expert** |
| `client/src/App.vue` | REWRITE template + style | **vue-expert** |
| `client/src/views/*.vue` | CSS-only (tokens) | **vue-expert** (one Task each) |

Use `AskUserQuestion`:

1. **Refresh depth** — Full visual refresh (default) / Sidebar-only / Tokens-only.
2. **Mobile drawer** — Off-canvas drawer (default) / Always-icons-only / Skip mobile for v1.

If "Sidebar-only", skip Step 7 (view token migrations). If "Tokens-only", skip Steps 5–7.

## Phase 3: Implement

Sequential. Do not parallelize — later steps depend on earlier ones.

### Step 1 — tokens.css (direct write)
Write `client/src/styles/tokens.css` verbatim from `templates/tokens.css`. No edits.

### Step 2 — main.js import (direct edit)
Add `import './styles/tokens.css'` after the last existing import in `client/src/main.js`.

### Step 3 — package.json + install (user-gated)
Edit `client/package.json` to add `"lucide-vue-next": "^0.400.0"` to `dependencies`. Then print:

> Run `cd client && npm install` to install lucide-vue-next. Reply "installed" when done.

**Pause for user confirmation. Do not auto-run npm install.**

### Step 4 — locales (direct edit, conditional)
If `nav.reports` is missing from `en.js` or `ja.js`, add it. English: `"Reports"`. Japanese: `"レポート"`. Mirror the existing key style.

### Step 5 — AppSidebar.vue (vue-expert delegation)
Fire a `vue-expert` Task with this prompt shape (see Delegation pattern below).

### Step 6 — App.vue rewrite (vue-expert delegation)
Fire one `vue-expert` Task. CRITICAL: must preserve every existing import, every modal mount, all `setup()` logic verbatim. Only the `<template>` shell and `<style>` block change.

### Step 7 — view migrations (vue-expert delegation, one per view)
For each `.vue` file under `client/src/views/` — fire a separate `vue-expert` Task. CSS-only diffs. One view per delegation so each diff is reviewable.

## Phase 4: Verify

See `PLAYBOOK.md § Verification` for the full Playwright sequence. Summary:

1. Confirm frontend at `localhost:3000`, backend at `localhost:8001`.
2. Desktop 1440×900: screenshot home, click each route, assert no console errors.
3. Token wiring sanity: `--space-4` resolves to `1rem`, `--color-accent` to `#2563eb`.
4. Sidebar collapse + persistence.
5. Mobile 375×812: off-canvas drawer + hamburger + auto-close on nav.

On fail: surface screenshots and console output. Do not auto-fix — remediation is a separate task.

## Delegation pattern (canonical)

Every `vue-expert` Task follows this shape:

```
INPUTS:
  template_path: <absolute path to skill template>
  target_path:   <absolute path to client/src/...>
  reference:     .claude/skills/redesign-vue-saas/DESIGN.md

HARD CONSTRAINTS:
  - Options API + setup() (NOT <script setup>)
  - Preserve i18n keys, router paths, modal mounts
  - All styles consume var(--*) tokens, no literal hex/rem
  - No emojis
  - Single file only — do not modify any other file

TASK BODY:
  <natural-language description of the specific change>
```

### Step 5 prompt (AppSidebar.vue)

```
Create `client/src/components/AppSidebar.vue` from the template at
`.claude/skills/redesign-vue-saas/templates/AppSidebar.vue`.

Constraints:
- Options API + setup() style (NOT <script setup>).
- Use the existing `useI18n` composable from `@/composables/useI18n`.
- Import LanguageSwitcher and ProfileMenu from `./` (same components dir).
- The 6 nav items + icon mapping is in the template; preserve exactly
  (or adapt to the actual route list discovered in Phase 1).
- Persist collapsed state to localStorage under key `sidebar-collapsed`.
- All styles consume `var(--*)` tokens from tokens.css. No literal hex/rem.
- No emojis.

Read `.claude/skills/redesign-vue-saas/DESIGN.md` for the sidebar visual spec
(dimensions, active state, mobile behavior).
```

### Step 6 prompt (App.vue rewrite)

```
Rewrite `client/src/App.vue` using the structure in
`.claude/skills/redesign-vue-saas/templates/AppShell.vue`.

CRITICAL — preserve:
- Every existing import (modals, components, composables, refs)
- All setup() logic verbatim (tasks, addTask, deleteTask, toggleTask,
  any computed, watch, lifecycle hooks)
- Every modal mount in the template
- The FilterBar mount, positioned between sidebar and view content

ONLY change:
- Replace the top-nav <header> block with <AppSidebar />
- Wrap content in the new .app-shell grid layout
- Replace the <style> block with token-based styles from the template

Read `.claude/skills/redesign-vue-saas/DESIGN.md` for the shell layout spec.
No emojis.
```

### Step 7 prompt (per-view token migration)

```
Update `client/src/views/{NAME}.vue` to consume design tokens.

ONLY change the scoped <style> block. Do NOT touch <template> or <script>.

Replace every literal value with the matching var(--*) from
`client/src/styles/tokens.css` per the mapping in
`.claude/skills/redesign-vue-saas/DESIGN.md`:
- All spacing (rem values) → --space-1 through --space-12
- All typography sizes → --text-xs through --text-3xl
- All colors → --color-* / --slate-*
- All radii → --radius-sm / --radius-md
- All shadows → --shadow-card-hover

Refer to `.claude/skills/redesign-vue-saas/templates/_example-view.vue`
for the target style pattern. No emojis.
```

## Bundled files

- `DESIGN.md` — design opinion (token scales, sidebar specs, component chrome, migration mapping)
- `PLAYBOOK.md` — discovery probes + Playwright verification recipe
- `templates/tokens.css` — CSS variable definitions (lifted verbatim)
- `templates/AppSidebar.vue` — sidebar component reference
- `templates/AppShell.vue` — new App.vue shell structure
- `templates/_example-view.vue` — view style migration target
