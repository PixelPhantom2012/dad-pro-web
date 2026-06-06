# Warm Premium Redesign — hagaiheshes.com

**Date:** 2026-06-06  
**Status:** Approved for implementation

---

## Goal

Complete visual redesign of `index.html` from dark-mode to a light, warm, premium aesthetic. The site should feel like a trusted senior advisor — personal, credible, human — not a startup or an agency. Everything stays in one file (vanilla HTML + `<style>` + IIFE `<script>`).

---

## Design Direction: Warm Premium Personal Brand

### Color Palette

| Token | Value | Usage |
|---|---|---|
| `--bg` | `#faf8f4` | Main background (warm off-white) |
| `--bg-alt` | `#f0e9de` | Alternate sections (clients, about) |
| `--ink` | `#1e1612` | Primary text, dark sections |
| `--text-body` | `#5a4a3a` | Body copy |
| `--text-dim` | `#7a6a58` | Secondary text |
| `--text-faint` | `#9a8a78` | Labels, captions |
| `--gold` | `#c9a96e` | Primary accent (eyebrows, CTAs, dots) |
| `--gold-warm` | `#b8975a` | Hover state for gold |
| `--line` | `#e8ddd0` | Borders and dividers |
| `--line-alt` | `#e0d6c8` | Borders on `--bg-alt` sections |

### Typography

- **Headings:** Gloock (serif) — keep existing Google Fonts link
- **Body / Labels:** Figtree (sans-serif) — keep existing Google Fonts link
- **Labels:** Figtree, 10px, weight 600, `letter-spacing: 0.16em`, uppercase, color `--gold`
- Hero title: `clamp(56px, 7vw, 96px)`, `line-height: 0.94`, `letter-spacing: -0.03em`
- Section titles: `clamp(28px, 3.5vw, 48px)`

### Crystal motif

Keep the faceted polygon SVG geometry unchanged. Recolor facet fills from `#b8975a` with existing opacities to the new warm palette — the polygon points stay identical. Used in: nav logo mark, contact section watermark (opacity 0.08).

---

## Page Structure

### 1. Nav
- Sticky, `height: 60px`
- Background: `#faf8f4` at 90% opacity with `backdrop-filter: blur(12px)`
- Border-bottom: `1px solid var(--line)`
- Left: logo mark (crystal SVG 24px) + "Hagai Heshes" in Gloock 14px
- Right: links (Work, Approach, About) + "Get in touch" CTA button (border `--gold`, text `--gold`)
- Mobile ≤720px: hamburger + drawer (same mechanism as current)

### 2. Hero
- `display: grid; grid-template-columns: 1fr 1fr`
- `min-height: calc(100svh - 60px)`
- **Left column (hero-text):** `padding: 72px 48px 72px 40px`
  - Eyebrow: gold line (32px) + "POSITIONING & GO-TO-MARKET" label
  - H1: "The" + italic "Crystallizer" in gold
  - Tagline: "You know what you've built. I make sure everyone else does too." — 15px, `--text-body`
  - Lede: "Positioning and go-to-market for companies…" — 13px, `--text-dim`
  - CTAs: `btn-primary` (gold fill, "Let's talk →") + `btn-ghost` (border `--line`, "See my work")
- **Right column (hero-photo):** background `--bg-alt`, `border-left: 1px solid var(--line)`
  - `img src="logos/hagai.jpeg"` — `width: 100%`, `aspect-ratio: 3/4`, `object-fit: cover`, `object-position: top center`
  - Italic name caption below: "Hagai Heshes" in Gloock, color `--gold`, 0.7 opacity
- Mobile ≤860px: stack vertically (photo on top, condensed); ≤720px: hide photo column entirely

### 3. Statement Strip
- Full-width, background `--ink` (`#1e1612`)
- `padding: 48px 40px`
- One centered serif quote, `font-size: clamp(18px, 2.2vw, 28px)`, color `#f0e9de`
- Em word in gold: *"The world is full of great products that never found their people. I fix* **that**."
- `border-top` and `border-bottom`: `1px solid rgba(255,255,255,0.06)`

### 4. Pillars (Services)
- Background: `--bg`
- `display: grid; grid-template-columns: 1fr 1fr`
- Each pillar: `padding: 52px 40px`, `border-right: 1px solid var(--line)`
- Pillar num: Gloock 11px, `--gold`, opacity 0.6
- Pillar title: Gloock `clamp(22px, 2.2vw, 30px)`, `--ink`
- Pillar body: Figtree 14px, `--text-dim`, `line-height: 1.8`
- Hover: `background: #f5f0e8` transition 0.2s
- Mobile ≤680px: stack vertically

### 5. Clients
- Background: `--bg-alt`
- Header row: "Selected clients" (Gloock 22px, `--ink`) + "PORTFOLIO" label — `padding: 48px 40px 28px`
- `display: grid; grid-template-columns: repeat(4, 1fr)` with border grid
- Each cell: `padding: 28px 20px`, logo `img` with `filter: grayscale(100%); opacity: 0.40`
- Hover: `opacity: 0.80; filter: grayscale(20%); background: var(--bg)`
- Mobile ≤600px: 2 columns

### 6. Deliverables
- Background: `--bg`
- `display: grid; grid-template-columns: 260px 1fr; gap: 80px; padding: 72px 40px`
- Left col: label + "What you walk away with" (Gloock)
- Right: 2-column `deliv-grid` — Crystallization | Go-to-Market
- Items: Figtree 13px, `--text-dim`, gold dot prefix, `border-bottom: 1px solid var(--line)`
- Mobile ≤720px: stack, single column lists

### 7. About
- Background: `--bg-alt`
- No photo (photo already in hero)
- `padding: 72px 40px`
- `display: grid; grid-template-columns: 1fr 1fr; gap: 64px`
- Left: label "About" + gold rule 32px + title "Twenty years of making things clear." (Gloock)
- Right: 2 paragraphs of bio copy (Figtree 15px, `--text-body`, `line-height: 1.85`)
- Mobile ≤720px: stack

### 8. Contact
- Background: `--ink`
- `padding: 88px 40px`
- `display: grid; grid-template-columns: 1fr auto; gap: 64px; align-items: center`
- Headline: Gloock `clamp(36px, 5vw, 68px)`, color `#f0e9de`, with italic gold "need"
- Sub: Figtree 14px, `--text-faint`
- CTAs: gold fill email button + ghost WhatsApp button
- Right: crystal SVG watermark, `opacity: 0.08`
- Mobile ≤860px: hide crystal, single column

### 9. Footer
- Background: `#140e08` (darkest)
- `padding: 22px 40px`
- `display: flex; justify-content: space-between`
- Copy: "© 2026 Hagai Heshes" — Figtree 11px, `--text-faint`
- Nav links: Work, Approach, About, Contact — same style

---

## Animations

Keep all existing animation mechanisms, retune for light background:

- **Hero entrance:** `body.loaded` class triggers — eyebrow slides in from left, title/tagline/lede/actions fade up with staggered delays
- **Scroll reveal:** `.reveal` (fade + translateY 24px), `.reveal-left` (translateX -20px), `.stagger` on client logos
- **Crystal float:** Remove from hero — photo replaces the crystal column entirely. Crystal remains only in: nav logo mark + contact section watermark (static, no animation).
- **Mouse parallax:** Remove from hero entirely.
- **Scroll-to-top button:** Keep, restyle to warm palette
- **Reduced motion:** Keep existing `prefers-reduced-motion` rule

---

## JavaScript

Keep the same IIFE with:
- `body.loaded` on window load
- IntersectionObserver for `.reveal`, `.reveal-left`, `.stagger`
- Mobile drawer toggle (burger → drawer)
- Scroll-to-top button visibility
- Active nav link highlighting via IntersectionObserver on sections
- Remove: crystal mouse parallax (photo doesn't need it)

---

## File Structure

No changes. Everything in `index.html`. Assets in `logos/`. No new dependencies.

### Logo files (confirm these exist):
- `logos/hagai.jpeg` — portrait photo for hero
- `logos/tytocare_logo-removebg-preview.png`
- `logos/quantalx_logo-removebg-preview.png`
- `logos/neurolief_logo-removebg-preview.png`
- `logos/Band-Logo.webp`
- `logos/wd_logo-removebg-preview.png`
- `logos/seagate_logo-removebg-preview.png`
- `logos/monday_logo-removebg-preview.png`
- `logos/wix_logo-removebg-preview.png`

---

## What Changes vs Current

| Current | New |
|---|---|
| Dark background (`oklch(13% …)`) | Warm off-white (`#faf8f4`) |
| Crystal SVG in hero right column | Hagai photo in hero right column |
| Crystal-focused brand | Person-forward brand |
| Mouse parallax on crystal | Removed |
| Dark sections throughout | One dark Statement strip + dark Contact |
| Gold on dark (subtle) | Gold on light (more prominent, warm) |
| `oklch()` color values | Hex values for simplicity |

## What Stays the Same

- Gloock + Figtree fonts
- Crystal geometry (polygon points unchanged)
- All section content/copy
- Mobile breakpoints and behavior
- Scroll animations mechanism
- Accessibility: focus-visible, aria-*, 44px touch targets, meaningful alt text
- Single-file architecture, no build step
