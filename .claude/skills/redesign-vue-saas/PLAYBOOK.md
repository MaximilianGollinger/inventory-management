# PLAYBOOK.md — Discovery probes and verification recipes

Mechanical recipes for the skill. Read by the orchestrator at execution time, not by vue-expert.

## Discovery probes

Each probe extracts a specific fact. Run in order; STOP on any blocker.

### 1. Vue version + framework detection

Read `client/package.json`:
- `dependencies.vue` must match `^3.x`. If `^2.x` or missing → STOP, surface "Not a Vue 3 project."
- Reject if any of these are in `dependencies` or `devDependencies`: `tailwindcss`, `primevue`, `vuetify`, `element-plus`, `naive-ui`, `quasar`. → STOP, surface "Project uses a CSS framework that conflicts with this skill's approach."
- Warn (don't STOP) if `vue-router < 4` — sidebar works either way but mention to user.

### 2. Router shape

Read `client/src/main.js` (and `client/src/router/*.js` if it exists separately):
- Extract every `{ path, component }` from the `routes` array.
- Record the order — sidebar nav follows route declaration order.
- Confirm `createWebHistory()` (not hash mode); if hash mode, warn but proceed.

### 3. App shell shape

Read `client/src/App.vue`:
- Find the top-nav header element.
- List every component mounted in the template (FilterBar, modals, LanguageSwitcher, ProfileMenu, etc.).
- Extract the `setup()` block and identify state to preserve (refs, computed, lifecycle hooks).

### 4. Views

`ls client/src/views/` → record file list. Every `.vue` file here gets the token migration pass in Phase 3 Step 7, even if not routed (e.g., a `Backlog.vue` that's rendered as a sub-view).

### 5. Components

`ls client/src/components/` → identify nav-affecting components vs modals. Modals stay put. `FilterBar.vue`, `LanguageSwitcher.vue`, `ProfileMenu.vue` slot into the new shell.

### 6. i18n

Read `client/src/composables/useI18n.js` — confirm the `t()` signature.
Read `client/src/locales/en.js` and `ja.js` — look for these keys:
- `nav.companyName`
- `nav.subtitle`
- `nav.overview`
- `nav.inventory`
- `nav.orders`
- `nav.finance`
- `nav.demandForecast`
- `nav.reports`

Any missing → list to user; Phase 3 Step 4 adds them. English defaults: "Reports". Japanese: "レポート".

### 7. Partial state probe

- `client/src/styles/tokens.css` exists? → diff against `templates/tokens.css`. If different, ask user: overwrite / keep existing / merge.
- `client/src/components/AppSidebar.vue` exists? → ask user: overwrite / skip.
- `client/src/App.vue` already imports `AppSidebar`? → skill has been partially applied; offer to resume from the next missing step.

## Verification recipe (Playwright)

Run after Phase 3 completes. Preconditions: frontend at `http://localhost:3000`, backend at `http://localhost:8001`. If servers are not up, ask user to start them — do NOT auto-start.

### Desktop (1440×900)

1. `mcp__playwright__browser_resize` width=1440 height=900.
2. `mcp__playwright__browser_navigate` url=`http://localhost:3000/`.
3. `mcp__playwright__browser_snapshot` — assert:
   - A `<aside>` or `[role="navigation"]` landmark exists.
   - It contains 6 nav links matching the routes.
   - The first nav link (Overview) has the active marker.
4. `mcp__playwright__browser_take_screenshot` filename=`redesign-desktop-overview.png` — visual confirm: sidebar on left (~240px), light background, light content area, 6 items with icons.
5. For each route in order: `mcp__playwright__browser_click` the nav link → `mcp__playwright__browser_snapshot` → assert active state moved → `mcp__playwright__browser_take_screenshot` filename=`redesign-desktop-{route}.png`.
6. `mcp__playwright__browser_console_messages` — assert no `error` level messages. `warn` tolerated.

### Token wiring sanity

7. `mcp__playwright__browser_evaluate` function:
   ```js
   () => getComputedStyle(document.documentElement).getPropertyValue('--space-4').trim()
   ```
   Expected: `1rem`.

8. `mcp__playwright__browser_evaluate` function:
   ```js
   () => getComputedStyle(document.documentElement).getPropertyValue('--color-accent').trim()
   ```
   Expected: `#2563eb`.

### Sidebar collapse

9. `mcp__playwright__browser_click` on the collapse toggle button.
10. `mcp__playwright__browser_evaluate`:
    ```js
    () => document.querySelector('.sidebar').classList.contains('collapsed')
    ```
    Expected: `true`.
11. `mcp__playwright__browser_take_screenshot` filename=`redesign-desktop-collapsed.png` — confirm icons-only mode, 64px width.
12. Reload page; `mcp__playwright__browser_evaluate` again — assert collapsed state persisted from localStorage.

### Mobile (375×812)

13. `mcp__playwright__browser_resize` width=375 height=812.
14. `mcp__playwright__browser_navigate` url=`http://localhost:3000/`. (Reset state.)
15. `mcp__playwright__browser_take_screenshot` filename=`redesign-mobile-overview.png` — confirm sidebar off-canvas (not visible), slim top bar with hamburger visible.
16. `mcp__playwright__browser_click` on the hamburger button.
17. `mcp__playwright__browser_take_screenshot` filename=`redesign-mobile-drawer-open.png` — confirm drawer slides in, backdrop visible.
18. `mcp__playwright__browser_click` on the "Inventory" link in the drawer.
19. `mcp__playwright__browser_snapshot` — assert URL is `/inventory`, drawer is closed.

## Pass / fail criteria

**Pass:** All 19 steps succeed. Zero console errors. All screenshots saved. Tokens resolve to expected values.

**Fail:**
- Any console error of level `error`.
- Any route fails to render (network error, blank page, exception).
- Sidebar doesn't collapse, or doesn't persist collapsed state.
- Token vars resolve to empty string (tokens.css not loaded).
- Mobile drawer fails to open or close.

**On fail:** print the failing step number, the failing assertion, the screenshot path, and the relevant console messages. Then STOP. Do not attempt auto-fix — that's a separate task.
