# Site Redesign — Section Rhythm & Visual Uplift

**Date:** 2026-06-10  
**Status:** Approved for implementation

## Problem

Three compounding issues make the site feel flat and unfinished:

1. **Crystal background visible** — `mix-blend-mode: multiply` is broken because `opacity` on `.hero-crystal-col` creates an isolated stacking context, preventing the multiply blend from reaching the page background.
2. **Monotonous visual rhythm** — every section shares the same white background and the same two-column grid layout, producing no visual breath or hierarchy between sections.
3. **"How I Work" layout** — a label-left / body-right two-column split that reads as an unfinished placeholder, not an intentional layout.

## Approach: Alternating Section Contrast

Keep the site light and bright overall, but introduce rhythm through background alternation and layout variation. No dark-site flip — only targeted dark sections where it earns its weight.

## Section-by-Section Changes

### Section Rhythm

| Section | Background | Change |
|---------|-----------|--------|
| Hero | `--bg` (warm off-white) | No change |
| Statement Strip | Dark indigo | No change (keep as-is) |
| Services / Pillars | `--indigo-bg` (light blue tint) | **New** |
| Clients | `--bg` | No change |
| How I Work | `--dark-bg` (dark navy) | **New — dark** |
| Deliverables | `--bg-alt` (slightly warm) | Minor tint |
| About | `--bg` | No change |
| Contact | Dark (existing) | No change |

### Fix 1: Crystal Background (P0 — Technical)

**Root cause:** `opacity: 0` on `.hero-crystal-col` creates a compositing group, breaking `mix-blend-mode: multiply` on the canvas child relative to the page background.

**Fix:**
- Remove `opacity` and `transition` from `.hero-crystal-col`
- Add `mix-blend-mode: multiply` to `.hero-crystal-col` (parent, no opacity)
- Move `opacity: 0 → 1` with `transition` to `.hero-crystal-canvas` (child)
- Update `body.loaded` rule to target `.hero-crystal-canvas` not `.hero-crystal-col`

### Fix 2: Services / Pillars Section (P1 — Design)

**Current:** Two-column card layout, small subtle "01 / 02" numbers.

**New:**
- Background: `var(--indigo-bg)` — very light blue tint, creates visual separation
- Section padding: 100px top/bottom (currently ~72px)
- Numbers "01" / "02": large Playfair Display serif, `--gold`, ~96px font-size — act as visual anchors not labels
- Title directly below number, no extra label
- Body text below title, max 48ch
- Remove any card/box borders; open layout, no containers around each pillar
- Grid: `1fr 1fr` on desktop, `1fr` on mobile (keep)

### Fix 3: How I Work Section (P1 — Design + Layout)

**Current:** Two-column grid: label/title on left, paragraphs on right. Reads as unfinished.

**New layout — dark, full-width, centered:**
- Background: `var(--dark-bg)` — dark navy
- Full-width, centered content, max-width 680px
- Heading "How I Work" — Playfair Display, large, white, centered, `font-style: italic`
- Content: three to four numbered statements, each on its own line
  - Gold number (`--gold`, Plus Jakarta Sans, 11px uppercase tracked)
  - Statement text below in white, 18px, `line-height: 1.7`
  - 32px gap between statements
- No two-column grid — purely vertical stacked list
- 100px top/bottom padding
- Remove `.how-label-col` entirely

### Fix 4: Gold Accent Reinforcement (P2 — Polish)

- Section dividers: replace `1px solid var(--line)` with `1px solid var(--gold)` on the Services → Clients boundary (one deliberate gold line, not all of them)
- Deliverables section: ensure list items have enough visual weight (larger type or gold left accent on numbers, not borders)

## Anti-Patterns to Avoid

- No side-stripe borders on pillar cards (already not there — keep it that way)
- No gradient backgrounds
- No identical card grids (the open pillar layout avoids this)
- No gradient text

## Scope

**In scope:**
- Crystal blend mode fix
- Services/Pillars background + number treatment
- How I Work full redesign (dark, vertical, centered)
- Deliverables minor tint update
- One gold section divider line

**Out of scope:**
- Nav, Statement Strip, Clients, About, Contact — no changes
- Copy — no changes
- Fonts — no changes
- Mobile breakpoints — update to match new layouts, no new breakpoints

## Success Criteria

- Crystal displays on the white hero background with no visible JPG bounding box
- Opening the page and scrolling feels like distinct sections, not one long white column
- "How I Work" is immediately readable as intentional and premium, not a layout draft
