# Bank MDA Custom Web Resource — Design System Spec

Single source of truth for styling ALL custom web resources / custom pages in the bank's Dataverse model-driven apps. Derived from the Logs Tab tiles (spec-1a / spec-1b). Any AI agent or developer building custom UI must follow this file; component-specific specs only define deltas.

## 1. Design principles

1. **Native-first**: custom UI must feel like part of the model-driven app, not a foreign website. Match Fluent/MDA conventions (Segoe UI, subtle borders, quiet greys).
2. **Red is a signal, not decoration**: brand red marks urgency, primary actions, links, and the active/hover state — never large background fills except the 3px brand rule.
3. **Text + shape, never color alone** for status meaning.
4. **Density over whitespace**: bank users scan; prefer compact tables/chips to hero layouts. No gradients, no emoji, no rounded-corner excess (max radius 6px, chips 10px).

## 2. Design tokens

### Color
| Token | Value | Use |
|---|---|---|
| `brand-red` | `#c8102e` | links, primary buttons, urgent status, active accents, current-week trend bar |
| `brand-red-hover` | `#a50d26` | link/button hover, "warning" text tier |
| `red-tint-bg` | `#fbeaed` | urgent chip/badge background |
| `red-tint-border` | `#f2c6cd` | urgent chip border |
| `rose-tint-bg` | `#fdf3f5` | mid-urgency ("at risk") chip bg, text `#a50d26` |
| `red-soft` | `#e08a99` | mid-urgency bar segment |
| `text-primary` | `#323130` | body text, values; also charcoal band bg |
| `text-secondary` | `#605e5c` | labels, secondary text |
| `text-meta` | `#8a8886` | timestamps, hints, row counts |
| `border` | `#e1dfdd` | card/input/table outer borders |
| `hairline` | `#edebe9` | inner dividers, table row borders |
| `hairline-soft` | `#f3f2f1` | footer hairlines, bar tracks, monogram bg |
| `grey-bar-1/2/3` | `#8a8886` / `#b8b6b4` / `#d9d7d5` | neutral chart/bar segments, urgency-graded |
| `bg-page` | `#f3f2f1` | page/canvas background (or transparent in a form tab) |
| `bg-subtle` | `#faf9f8` | table header rows, inset strips, hover rows |
| `white` | `#ffffff` | card/surface background |

Need more colors (charts)? Derive with oklch from `#c8102e` and the grey ramp; never introduce new hues (no blue/green/orange semantics — urgency is red→grey).

### Typography (Segoe UI, system-ui fallback)
| Role | Size/weight |
|---|---|
| Page title | 16px / 600 |
| Section/card title | 14px / 600 |
| Band title (uppercase) | 13px / 600, letter-spacing 0.3px |
| Big stat number | 34px / 600 (compact contexts 22–28px) |
| Body / table cells | 12.5px / 400 |
| Labels, legends | 12px / 400, `text-secondary` |
| Chips, badges | 11px / 600 |
| Meta, footnotes | 11–11.5px / 400, `text-meta` |
Minimum text size: 11px. Numbers use `line-height:1` when displayed as stats.

### Spacing & shape
- Spacing scale: 4 / 6 / 8 / 10 / 12 / 14 / 16 / 18 px. Card padding `16px 18px` (dense: `12px 14px`). Grid gap between cards 16px.
- Radius: cards/inputs 6px, small elements 4px, chips/pills 10px, avatars 50%.
- Borders: 1px everywhere; the only thick line is the 3px `brand-red` accent rule.
- Shadows: resting `0 1px 2px rgba(0,0,0,0.04)`; hover `0 2px 8px rgba(200,16,46,0.12)` (red-tinted, with `border-color:brand-red`) or neutral `0 3px 10px rgba(0,0,0,0.14)` for band-style cards. Nothing heavier.

## 3. Core components

### Card / tile
White bg, `1px solid border`, radius 6px. Clickable cards: `cursor:pointer`, hover per above, entire card = one action; inner links stop propagation. Equal-height rows: `align-items:stretch` + `margin-top:auto` footers.

### Charcoal header band (for "important object" cards & panel headers)
Bg `#323130`, white text, padding `12px 16px`, `border-bottom:3px solid brand-red`, uppercase 13px/600 title, right-aligned stat.

### Chips / status badges
11px/600, padding `2-3px 9px`, radius 10px. Tiers:
- Urgent: `red-tint-bg` / `brand-red` text / `red-tint-border`
- Warning: `rose-tint-bg` / `#a50d26` text
- Neutral graded: bg `#f3f2f1` or `#edebe9`, text `#323130` → `#605e5c` → `#8a8886` (more done = lighter)
Hide zero-count chips.

### Buttons
- Primary: bg `brand-red`, white text, 12–13px/600, padding `6px 14px`, radius 4px; hover `brand-red-hover`
- Secondary/toolbar: transparent bg, text `#605e5c` (or red 600 for the lead action), padding `6px 10px`, radius 4px; hover bg `#f3f2f1` (red actions: `#fbeaed`)
- Never outlined-red ghost buttons; never disabled-looking grey primaries.

### Links
`brand-red`, no underline; hover `brand-red-hover` + underline. Row-level nav affordance: "Open view ↗" / "Open X view →".

### Inputs
1px `border`, radius 4px, padding `5px 10px`, 12px text, `outline-color: brand-red` on focus. Placeholders in `text-meta`.

### Tables / grids (read-only lists)
- Header row: bg `bg-subtle`, 12px/600 `text-secondary`, left-aligned, padding `8px 12px`
- Body rows: 12.5px, `border-top:1px solid hairline`, hover bg `bg-subtle`; first data column is a red link
- Footer strip: 11.5px `text-meta`, "Rows: X of Y" left, "See all records ↗" right
- Toolbar above: 13px/600 title + text buttons + right-aligned filter input

### Avatars
Initials circles, white 700 text, `2px solid #fff`, stacked with negative margin (-6/-7px), max 3 + "+N". Bg cycle: `brand-red`, `#605e5c`, `#8a8886`.

### Stat block
Big number + 12px `text-secondary` label, bottom-aligned in a flex row.

### Distribution bar
6px tall, radius 3px, track `hairline-soft`; segments % of total, urgent→done: `brand-red`, `red-soft`, `#8a8886`, `#b8b6b4`, `#d9d7d5`. Legend: 8px dots + "Label · N", 12px.

### Mini trend (sparkbar)
8 weekly bars, 5–6px wide, 2px gap, max height 20–26px; oldest bars `#d9d7d5`→`#b8b6b4`, current period `brand-red`. Always add a title/tooltip explaining the range.

### Monogram
28×28 (or 32×32), radius 4px, 13px/700 letter. Quiet: bg `hairline-soft`, letter `text-secondary`. Loud (row leads): bg `brand-red`/`#605e5c`/`#8a8886`, white letter.

### Alert / callout strip
Inset: bg `bg-subtle`, `1px solid hairline`, radius 4px, padding `7px 10px`, 12px. Urgent variant: red-tint set.

### Page header
16px/600 title + 12px `text-secondary` subtitle on one baseline row, right-aligned meta ("Refreshed HH:MM"). Margin-bottom 14px.

### Empty state
No illustrations. 12px `text-secondary` line ("No related records") + keep the primary nav link. Hide charts/chips at zero.

## 4. Interaction & behavior

- Hover states on everything clickable (rows, cards, buttons); transitions ≤ 150ms, opacity/shadow/border only — no movement/scale animations.
- Overview surfaces are read-only; editing happens in standard MDA forms/subgrids via `Xrm.Navigation` (navigateTo entitylist / openForm).
- Keyboard: clickable cards get `role="link"` + `tabindex="0"` + Enter/Space; focus visible via red outline.
- Relative timestamps ("2 d ago" compact, "2 days ago" in labeled grids); absolute dates as "18 Aug 2026". Overdue/late dates: `brand-red` + 600.
- Loading: grey skeleton blocks (`#edebe9`, radius 4px), no spinners for < 1s fetches.

## 5. Layout rules

- Responsive via flex-wrap: cards `flex:1; min-width:300px`, gap 16px. Content max-width 1400px.
- Always flex/grid with `gap`; never margin-chained siblings.
- Data via `Xrm.WebApi.retrieveMultiple` (aggregate FetchXML for counts); one fetch per card, render skeletons meanwhile.

## 6. Reference implementations

- `Logs Tab Web Resource.dc.html` — tile variations 1a/1b/1c (live examples of most components)
- `spec-1a-card-overview.md`, `spec-1b-header-band.md` — component-level specs following this system
