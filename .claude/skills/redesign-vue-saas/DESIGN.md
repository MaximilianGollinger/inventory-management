# DESIGN.md — Visual opinion for `redesign-vue-saas`

The single source of truth for the visual refresh. Vue-expert reads this before every delegation. Update here when tokens change — never inline values in templates or Task prompts.

## Philosophy

Light sidebar + light content (Vercel / Linear / Stripe). Slate-based neutral palette. 4px spacing grid. Inter typeface. Subtle borders, soft shadows on hover only. No emojis.

## Spacing scale (4px base)

| Variable | Value | Usage |
|---|---|---|
| `--space-1` | 0.25rem (4px) | hairline gaps, icon-text padding |
| `--space-2` | 0.5rem (8px) | tight padding, badge insets |
| `--space-3` | 0.75rem (12px) | sidebar nav item horizontal padding |
| `--space-4` | 1rem (16px) | default block gap, button padding |
| `--space-5` | 1.25rem (20px) | card internal padding |
| `--space-6` | 1.5rem (24px) | section gap |
| `--space-8` | 2rem (32px) | content edge padding |
| `--space-10` | 2.5rem (40px) | view top padding |
| `--space-12` | 3rem (48px) | major section break |

## Typography scale

| Variable | Size / line-height | Usage |
|---|---|---|
| `--text-xs` | 0.75rem / 1rem | table headers (uppercase), badges |
| `--text-sm` | 0.875rem / 1.25rem | table body, secondary labels, nav items |
| `--text-base` | 0.9375rem / 1.5rem | default body (preserves current) |
| `--text-lg` | 1.125rem / 1.625rem | card titles |
| `--text-xl` | 1.375rem / 1.75rem | logo, sidebar brand |
| `--text-2xl` | 1.875rem / 2.25rem | page H2 (preserves current) |
| `--text-3xl` | 2.25rem / 2.5rem | stat values (preserves current) |

Weights: `--font-medium: 500`, `--font-semibold: 600`, `--font-bold: 700`.
Tracking: `--tracking-tight: -0.025em` on display sizes (≥ `--text-xl`), `--tracking-wide: 0.05em` for uppercase labels.
Font stack: `Inter, system-ui, -apple-system, 'Segoe UI', sans-serif`.

## Color tokens

### Slate scale (Tailwind values, formalized)

| Variable | Hex |
|---|---|
| `--slate-50` | #f8fafc |
| `--slate-100` | #f1f5f9 |
| `--slate-200` | #e2e8f0 |
| `--slate-300` | #cbd5e1 |
| `--slate-400` | #94a3b8 |
| `--slate-500` | #64748b |
| `--slate-600` | #475569 |
| `--slate-700` | #334155 |
| `--slate-800` | #1e293b |
| `--slate-900` | #0f172a |
| `--slate-950` | #020617 |

### Semantic neutrals

| Variable | Resolves to | Usage |
|---|---|---|
| `--color-bg` | `var(--slate-50)` | app background |
| `--color-surface` | `#ffffff` | cards, sidebar, modals |
| `--color-border` | `var(--slate-200)` | hairlines, card borders |
| `--color-text-primary` | `var(--slate-900)` | body, headings |
| `--color-text-secondary` | `var(--slate-500)` | metadata, captions |
| `--color-text-muted` | `var(--slate-400)` | placeholders, disabled |

### Accent (brand blue, preserved)

| Variable | Hex |
|---|---|
| `--color-accent` | #2563eb |
| `--color-accent-hover` | #1d4ed8 |
| `--color-accent-bg` | #eff6ff |

### Status (preserved)

| Variable | Hex |
|---|---|
| `--color-success` | #059669 |
| `--color-warning` | #ea580c |
| `--color-danger` | #dc2626 |
| `--color-info` | #2563eb |

## Sidebar specs

- **Width expanded:** 240px (`--sidebar-width`)
- **Width collapsed:** 64px (`--sidebar-width-collapsed`)
- **Background:** `var(--color-surface)`
- **Border-right:** `1px solid var(--color-border)`
- **Brand block:** 56px min-height, `var(--space-5)` padding. Logo `var(--text-xl)` `var(--font-bold)` `var(--tracking-tight)`. Subtitle `var(--text-xs)` `var(--color-text-secondary)`.
- **Nav item:**
  - Height 36px
  - Padding `var(--space-2) var(--space-3)`
  - Gap `var(--space-3)` between icon and label
  - Border-radius `var(--radius-sm)` (6px)
  - Default: `color: var(--color-text-secondary)`
  - Hover: `background: var(--slate-50)`, `color: var(--color-text-primary)`
  - Active (`.router-link-active`, `.router-link-exact-active`): `background: var(--color-accent-bg)`, `color: var(--color-accent)`
  - Transition: `background-color var(--transition-base), color var(--transition-base)`
- **Footer block:** `LanguageSwitcher` (hidden when collapsed), `ProfileMenu`, collapse-toggle button. Separated from nav by `1px solid var(--color-border)` and `var(--space-3)` padding-top.
- **Collapsed state:**
  - Labels hidden, icons centered
  - Brand subtitle hidden
  - LanguageSwitcher hidden (collapse toggle still visible)
  - Hover tooltip via `title` attribute (browser default for v1)
- **Persistence:** `localStorage` key `sidebar-collapsed`, value `"true"` or `"false"`. Default `false`. Read in `onMounted`.
- **Mobile (`<768px`):**
  - Sidebar becomes off-canvas drawer (`transform: translateX(-100%)`)
  - Slim sticky top bar (40px, `--sidebar-mobile-topbar-height`): hamburger left, brand center
  - Hamburger toggles drawer
  - Drawer auto-closes when a nav item is clicked
  - Backdrop overlay at 50% black, click to close

## Card chrome

- Background: `var(--color-surface)`
- Border: `1px solid var(--color-border)`
- Radius: `var(--radius-md)` (10px)
- Padding: `var(--space-5)`
- Default shadow: none
- Hover shadow: `var(--shadow-card-hover)` = `0 1px 2px rgba(15,23,42,.04), 0 4px 12px rgba(15,23,42,.06)`
- Transition: `box-shadow var(--transition-base)`
- Section divider inside card: `1px solid var(--slate-100)`, `var(--space-4)` padding

## Buttons

The project has no unified button today. Establish three variants:

### Primary
- Background `var(--color-accent)`, color `#fff`
- Padding `var(--space-2) var(--space-4)`
- Radius `var(--radius-sm)`
- Font `var(--text-sm)` `var(--font-medium)`
- Hover: `background: var(--color-accent-hover)`
- Transition: `background-color var(--transition-base)`

### Secondary
- Background `#fff`, color `var(--slate-700)`, border `1px solid var(--color-border)`
- Same dimensions as primary
- Hover: `background: var(--slate-50)`, `border-color: var(--slate-300)`

### Disabled
- `opacity: 0.5`, `cursor: not-allowed`, no hover state

## Inputs / selects

- Height 36px
- Padding `var(--space-2) var(--space-3)`
- Border `1px solid var(--color-border)`, radius `var(--radius-sm)`
- Font `var(--text-sm)`
- Focus: `outline: none`, `border-color: var(--color-accent)`, `box-shadow: 0 0 0 3px rgba(37,99,235,.15)`
- Disabled: `background: var(--slate-50)`, `color: var(--color-text-muted)`

## Tables

Preserve current structure. Migrate values to tokens:
- `thead`: `background: var(--slate-50)`, `border-bottom: 1px solid var(--color-border)`
- `th`: `font-weight: var(--font-medium)`, `font-size: var(--text-xs)`, `text-transform: uppercase`, `letter-spacing: var(--tracking-wide)`, `color: var(--color-text-secondary)`, padding `var(--space-3) var(--space-4)`
- `td`: `font-size: var(--text-sm)`, padding `var(--space-3) var(--space-4)`, `border-bottom: 1px solid var(--slate-100)`
- Row hover: `background: var(--slate-50)`

## Radii, shadows, transitions

| Variable | Value |
|---|---|
| `--radius-sm` | 6px |
| `--radius-md` | 10px |
| `--radius-lg` | 16px |
| `--radius-full` | 9999px |
| `--shadow-sm` | `0 1px 2px rgba(15,23,42,.05)` |
| `--shadow-card-hover` | `0 1px 2px rgba(15,23,42,.04), 0 4px 12px rgba(15,23,42,.06)` |
| `--shadow-modal` | `0 20px 50px rgba(15,23,42,.15)` |
| `--transition-base` | `150ms ease-out` |
| `--transition-slow` | `250ms ease-out` |

## Z-index scale

| Variable | Value |
|---|---|
| `--z-sidebar` | 30 |
| `--z-mobile-topbar` | 40 |
| `--z-drawer-backdrop` | 45 |
| `--z-modal` | 50 |
| `--z-toast` | 60 |

## Out of scope (v1)

- **Dark mode.** Tokens are structured to make it a future drop-in via `:root[data-theme="dark"]` overrides. Do NOT implement, document, or test dark mode in v1.
- **Chart SVG internals.** Each view's chart code stays untouched. Only their containers / wrappers migrate.
- **Modal internals.** Modal chrome (overlay, panel, close) gets token treatment. Modal contents stay as-is.
- **Business logic.** No `setup()` changes, no API client changes, no data flow changes.

## Migration mapping cheat-sheet

When vue-expert migrates a view, replace literal values per this table:

| From | To |
|---|---|
| `padding: 1.25rem` | `padding: var(--space-5)` |
| `padding: 1rem` | `padding: var(--space-4)` |
| `padding: 0.75rem` | `padding: var(--space-3)` |
| `padding: 0.5rem` | `padding: var(--space-2)` |
| `padding: 2rem` | `padding: var(--space-8)` |
| `gap: 1rem` | `gap: var(--space-4)` |
| `gap: 1.5rem` | `gap: var(--space-6)` |
| `gap: 0.5rem` | `gap: var(--space-2)` |
| `color: #0f172a` | `color: var(--color-text-primary)` |
| `color: #64748b` | `color: var(--color-text-secondary)` |
| `color: #94a3b8` | `color: var(--color-text-muted)` |
| `background: #f8fafc` | `background: var(--color-bg)` |
| `background: #ffffff` / `background: white` | `background: var(--color-surface)` |
| `border: 1px solid #e2e8f0` | `border: 1px solid var(--color-border)` |
| `color: #2563eb` | `color: var(--color-accent)` |
| `background: #eff6ff` | `background: var(--color-accent-bg)` |
| `color: #059669` | `color: var(--color-success)` |
| `color: #dc2626` | `color: var(--color-danger)` |
| `color: #ea580c` | `color: var(--color-warning)` |
| `font-size: 0.75rem` | `font-size: var(--text-xs)` |
| `font-size: 0.875rem` | `font-size: var(--text-sm)` |
| `font-size: 0.9375rem` / `0.938rem` | `font-size: var(--text-base)` |
| `font-size: 1.125rem` | `font-size: var(--text-lg)` |
| `font-size: 1.375rem` | `font-size: var(--text-xl)` |
| `font-size: 1.875rem` | `font-size: var(--text-2xl)` |
| `font-size: 2.25rem` | `font-size: var(--text-3xl)` |
| `font-weight: 500` | `font-weight: var(--font-medium)` |
| `font-weight: 600` | `font-weight: var(--font-semibold)` |
| `font-weight: 700` | `font-weight: var(--font-bold)` |
| `letter-spacing: -0.025em` | `letter-spacing: var(--tracking-tight)` |
| `border-radius: 10px` | `border-radius: var(--radius-md)` |
| `border-radius: 6px` | `border-radius: var(--radius-sm)` |
| `transition: 150ms ease-out` (any prop) | use `var(--transition-base)` |

**If a value doesn't map exactly, escalate to the user — do not invent a new token.**
