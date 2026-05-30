# Feature Spec: Hero, Clients, About & Spacing Refresh
**Status:** DRAFT — awaiting approval

## 1. Goal
Tighten and de-clutter the single-page consulting site so it reads as confident and intentional rather than sparse — by enlarging the hero crystal, replacing the clients block with a clean static logo grid, stripping low-value labels/sections, and reducing excessive vertical whitespace site-wide.

**User story:** As a prospective client visiting Hagai Heshes' site, I want a focused, visually balanced page that gets to the point so that I trust the brand and reach the contact section quickly.

## 2. Acceptance criteria
- [ ] AC-1: The hero crystal SVG renders visibly larger than today (~25–35% larger) and the crystal column fills its right half with no large empty gap.
- [ ] AC-2: The "Hagai Heshes" crystal caption no longer appears anywhere in the hero.
- [ ] AC-3: The "Positioning & Go-to-Market" eyebrow label no longer appears in the hero.
- [ ] AC-4: The clients section shows a single static logo grid: 4 columns × 2 rows (8 logos) on desktop, with thin cell borders, greyscale logos of uniform height, and no company-name text or descriptions.
- [ ] AC-5: The old `.clients-grid` text grid, the `.clients-mobile` list, and the `.logo-strip` marquee are fully removed (HTML + CSS + related JS-irrelevant rules).
- [ ] AC-6: No section displays a number label (`§ 01`–`§ 06`, the `.pillar-num` "01"/"02", or "§ 06: Contact" prefix). All such labels are removed.
- [ ] AC-7: The About section no longer shows the Leanovation paragraph, the `.about-tags`, or the `.about-badge`.
- [ ] AC-8: The About photo displays in full color with no greyscale filter and no grayscale hover transition.
- [ ] AC-9: The entire Media/Press section (markup + CSS) is removed, and "Press" links no longer point to a non-existent `#media` anchor.
- [ ] AC-10: Vertical section padding is reduced by roughly 25–30% across all sections; the page feels tighter with no functional or layout breakage on desktop or mobile.
- [ ] AC-11: Color palette, fonts, crystal SVG geometry, and all preserved copy remain unchanged.

## 3. UI components
Single-file site (`index.html`); "components" below are markup blocks + their scoped CSS.

### 3.1 Hero crystal column — MODIFIED
- File: `index.html` — `.hero-crystal-col` / `.crystal-container` / `.crystal-svg`
- Remove the `.crystal-caption` block entirely (the "Hagai Heshes" text under the crystal).
- Enlarge the crystal: change the inline SVG `width="220" height="288"` to approximately `width="300" height="394"` (keep the `viewBox="0 0 260 340"` and all polygon geometry unchanged — scaling via width/height preserves the design).
- Adjust `.crystal-glow` size up proportionally (e.g. `300px` → `360px`) so the glow still frames the larger crystal.
- Reduce the asymmetric whitespace: the crystal column currently has `padding: 80px 32px 80px 48px`. Reduce to balanced, smaller padding (e.g. `56px 32px`) and ensure the column centers its content so the right side does not read as empty. States: idle float animation, hover drop-shadow, mouse-parallax (all preserved).

### 3.2 Hero eyebrow — MODIFIED
- File: `index.html` — `.hero-eyebrow` block inside `.hero-text`
- Remove the `<span class="t-label">Positioning &amp; Go-to-Market</span>`. Per requirements the "Positioning & Go-to-Market" label is removed. Remove the full `.hero-eyebrow` wrapper (line + label) since the line alone is meaningless. Note: the hero entrance CSS references `.hero-eyebrow` — see §10.

### 3.3 Clients logo grid — NEW (replaces grid + marquee)
- File: `index.html` — inside `<section class="clients-section" id="work">`
- New container `.clients-logos` with 8 `.client-logo` cells, each holding one `<img>` from `logos/`.
- Layout: `grid-template-columns: repeat(4, 1fr)` desktop → `repeat(2, 1fr)` tablet → `repeat(2, 1fr)` or `1fr` on small mobile (see §7).
- Cell styling: thin 1px borders (`var(--line)`), centered logo, consistent vertical padding, uniform logo height (e.g. `img { height: 40px; width: auto; object-fit: contain; }`).
- Logos greyscale: `filter: grayscale(100%); opacity: 0.65;`. Optional hover-to-color is allowed but NOT required; keep it simple and static. No text labels.
- Keep `.clients-header` ("Selected clients" + "B2B & Enterprise" label) — only the body below it changes.

### 3.4 Section headings — MODIFIED
- File: `index.html` — `.pillar-num`, `.how-label-col .section-num`, About `.section-num`, Contact `.t-label` "§ 06: Contact"
- Remove all numeric/section-number elements; keep the section titles themselves.

### 3.5 About block — MODIFIED
- File: `index.html` — `.about-section`
- Remove `.about-badge`, the Leanovation `<p>`, and the `.about-tags` block.
- Photo (`.about-photo-wrap img`): remove `filter: grayscale(15%)` and the `:hover` filter/scale change so it shows natural full color. Keep border-radius and border.

## 4. Logic & state
No JavaScript behavior changes required. Verify after edits:
- The hero mouse-parallax script still targets `#hero-crystal` (unchanged).
- The reveal/`IntersectionObserver` selectors still match remaining elements; removed elements simply have fewer observers.
- Active-nav-link observer iterates `section[id]`; removing the `#media` section is safe.

## 5. Data model
None. Static HTML/CSS site, no types or DB.

## 6. Routing
In-page anchor links only.
- Remove the `#media` "Press" link from the desktop `.nav-links`, the `.mobile-drawer`, and the `.footer-nav` (the target section is being deleted; leaving a dead anchor is a defect).
- All remaining anchors (`#work`, `#how`, `#about`, `#contact`) stay.

## 7. Edge cases & error states
| Scenario | Expected behavior |
|---|---|
| Logo image fails to load | Cell keeps its border and height; `alt` text (company name) shows. Each `<img>` retains a meaningful `alt`. |
| Logos differ in intrinsic aspect ratio | All constrained to a fixed `height` with `width:auto; object-fit:contain;` so heights match; widths vary naturally and stay centered. |
| Mobile (≤720px) clients view | Logo grid stays visible (no longer hidden). Old `.clients-mobile` removed. Use `repeat(2,1fr)` so 8 logos form 4 rows. |
| Very small screens (≤480px) | Logo grid remains `repeat(2,1fr)`; logos may shrink height slightly if needed to fit. |
| Hero on mobile (≤720px) | `.hero-crystal-col` is currently `display:none` on mobile — keep that behavior; the enlarged desktop crystal must not break mobile. |
| `prefers-reduced-motion` | Existing reduce-motion rule still applies; no new animations introduced. |
| Reduced padding | Ensure no section's content touches borders; min readable spacing retained on mobile. |

## 8. Security considerations
- [x] N/A — static marketing site, no auth, no endpoints, no user data.
- [ ] Keep `rel="noopener noreferrer"` on any retained external links (contact email/WhatsApp).
- [ ] No inline secrets or sensitive data introduced.

## 9. Files to create
None. All changes are within the existing `index.html`. (`logos/` images already exist.)

## 10. Files to modify
| File | Change |
|---|---|
| `index.html` — hero markup | Remove `.crystal-caption`; remove `.hero-eyebrow` (line + "Positioning & Go-to-Market" label); enlarge crystal SVG `width`/`height`. |
| `index.html` — hero CSS | Enlarge `.crystal-glow`; reduce/balance `.hero-crystal-col` padding to fix right-side whitespace; remove now-unused `.crystal-caption` rules. |
| `index.html` — hero entrance CSS | Remove `.hero-eyebrow` from the `.hero-text .hero-eyebrow {…}` opacity rule and the `body.loaded .hero-text .hero-eyebrow {…}` delay rule (selector now dangling). Keep title/tagline/lede/actions entrance. |
| `index.html` — clients markup | Delete `.clients-grid`, `.clients-mobile`, and `.logo-strip` blocks; insert new `.clients-logos` 4×2 static logo grid using the 8 `logos/` images. |
| `index.html` — clients CSS | Delete `.clients-grid`, `.client-cell`, `.logo-strip`, `.logo-track`, `.logo-item`, `.clients-mobile*` rules and their `@media` variants; add `.clients-logos` + `.client-logo` rules with responsive breakpoints. |
| `index.html` — section numbers | Remove `.pillar-num` elements (and `.pillar:hover .pillar-num` CSS), `.how-label-col .section-num` (How + Deliverables), About `.section-num`, and Contact "§ 06: Contact" prefix (keep "Contact" wording or drop the label per visual judgment). |
| `index.html` — about markup | Remove `.about-badge`, Leanovation `<p>`, and `.about-tags`. |
| `index.html` — about CSS | Remove `filter: grayscale(15%)` and the `.about-photo-wrap:hover img` filter/scale; remove now-unused `.about-badge`, `.about-tag`, `.about-tags` rules and the mobile `.about-tag` touch rule. |
| `index.html` — media section | Delete the entire `<section class="media-section" id="media">` block and all `.media-*` CSS (including its `@media` rules). |
| `index.html` — nav/drawer/footer | Remove the `#media` "Press" links from `.nav-links`, `.mobile-drawer`, and `.footer-nav`. |
| `index.html` — global spacing | Reduce vertical padding ~25–30% on: `.hero-text`/`.hero-crystal-col`, `.pillar`, `.clients-header`, `.how-inner`, `.deliv-inner`, `.about-inner`, `.contact-inner`, and `.section-header`. Keep horizontal padding and mobile paddings sensible. |

## 11. Out of scope
- Color palette, fonts, and crystal SVG geometry (only its render size changes).
- Hero text content, pillars copy, How I Work copy, What You Walk Away With copy, Contact, Footer copy.
- Mobile-specific CSS already in place (beyond removing rules tied to deleted elements).
- Accessibility improvements already present (focus states, contrast, touch targets, cursor) — preserve them.
- Any new sections, content, or data sources.

## 12. Open questions
- Crystal target size: spec proposes `width≈300 / height≈394`. Confirm this magnitude or specify "fill the column edge-to-edge."
- Clients header: keep the "Selected clients" heading + "B2B & Enterprise" label above the new logo grid, or drop them for a pure logo band?
- Contact "§ 06: Contact" label: remove only the "§ 06:" number and keep "Contact", or remove the whole label line?
- Logo hover: static greyscale only (recommended), or greyscale → color on hover like the old marquee?
