# Site Redesign — Section Rhythm & Visual Uplift Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Fix crystal blend mode bug and give the site visual rhythm via alternating section backgrounds and a full How I Work redesign.

**Architecture:** Single-file vanilla HTML/CSS/JS in `index.html`. All changes are CSS rewrites + one HTML restructure (How I Work section). No build step.

**Tech Stack:** Vanilla HTML5, CSS custom properties, vanilla JS (no frameworks)

---

### Task 1: Fix Crystal Blend Mode

**Files:**
- Modify: `index.html` — CSS block (lines ~386–403) and reduced-motion block (lines ~951–960)

The problem: `.hero-crystal-col` has `opacity: 0` which creates an isolated compositing group, so `mix-blend-mode: multiply` on the canvas child blends against that group's transparent backdrop instead of the page background, making the white JPG background visible.

Fix: move `mix-blend-mode` to the parent (no opacity), move `opacity` transition to the canvas child.

- [ ] **Step 1: Update `.hero-crystal-col` CSS**

Replace this block in the `<style>` tag:
```css
/* Crystal column */
.hero-crystal-col {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  overflow: visible;
  opacity: 0;
  transition: opacity 0.8s var(--ease-out) 0.1s;
}
body.loaded .hero-crystal-col { opacity: 1; }

.hero-crystal-canvas {
  display: block;
  height: clamp(400px, calc(100svh - var(--nav-h) - 40px), 880px);
  width: auto;
  mix-blend-mode: multiply;
  image-rendering: auto;
}
```

With:
```css
/* Crystal column */
.hero-crystal-col {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  overflow: visible;
  mix-blend-mode: multiply;
}

.hero-crystal-canvas {
  display: block;
  height: clamp(400px, calc(100svh - var(--nav-h) - 40px), 880px);
  width: auto;
  opacity: 0;
  transition: opacity 0.8s var(--ease-out) 0.1s;
  image-rendering: auto;
}
body.loaded .hero-crystal-canvas { opacity: 1; }
```

- [ ] **Step 2: Update reduced-motion rule**

Find this in the `@media (prefers-reduced-motion: reduce)` block:
```css
.hero-title-word-inner,
.hero-tagline-word-inner,
.hero-lede,
.hero-actions,
.hero-badge,
.hero-crystal-col,
.nav {
  opacity: 1 !important;
  transform: none !important;
}
```

Replace `.hero-crystal-col` with `.hero-crystal-canvas`:
```css
.hero-title-word-inner,
.hero-tagline-word-inner,
.hero-lede,
.hero-actions,
.hero-badge,
.hero-crystal-canvas,
.nav {
  opacity: 1 !important;
  transform: none !important;
}
```

- [ ] **Step 3: Visual verify**

Open `index.html` in a browser. The crystal animation should play with no visible white rectangle/bounding box — the crystal shape should appear to float directly on the white hero background.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "fix: crystal blend mode — move opacity to canvas child to avoid stacking context"
```

---

### Task 2: Services / Pillars Redesign

**Files:**
- Modify: `index.html` — CSS block for `.pillars-section` (lines ~487–544)

Make the numbers large gold Playfair Display anchors, add indigo-bg tint to the section.

- [ ] **Step 1: Replace `.pillars-section` and `.pillar-num` CSS**

Find and replace the entire pillars CSS block:
```css
/* ── PILLARS ── */
.pillars-section {
  background: var(--bg);
  border-bottom: 1px solid var(--line);
}
.pillars-inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  max-width: var(--maxw);
  margin: 0 auto;
}
.pillar {
  padding: 72px 56px;
  border-right: 1px solid var(--line);
  position: relative;
}
.pillar:last-child { border-right: none; }
.pillar::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
  background: var(--indigo);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.32s var(--ease-spring);
}
.pillar:hover::before { transform: scaleX(1); }
.pillar-num {
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--indigo);
  opacity: 0.55;
  margin-bottom: 28px;
  display: block;
}
.pillar-title {
  font-size: clamp(26px, 2.4vw, 34px);
  font-weight: 700;
  letter-spacing: -0.04em;
  color: var(--navy);
  margin-bottom: 16px;
  line-height: 1.05;
}
.pillar-body {
  font-size: 15px;
  line-height: 1.75;
  color: var(--text-dim);
  max-width: 36ch;
}

@media (max-width: 680px) {
  .pillars-inner { grid-template-columns: 1fr; }
  .pillar { border-right: none; border-bottom: 1px solid var(--line); padding: 44px 20px; }
  .pillar:last-child { border-bottom: none; }
}
```

With:
```css
/* ── PILLARS ── */
.pillars-section {
  background: var(--indigo-bg);
  border-bottom: 1px solid var(--line);
}
.pillars-inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  max-width: var(--maxw);
  margin: 0 auto;
}
.pillar {
  padding: 96px 56px;
  border-right: 1px solid var(--line);
  position: relative;
}
.pillar:last-child { border-right: none; }
.pillar-num {
  font-family: 'Playfair Display', Georgia, serif;
  font-size: clamp(72px, 8vw, 96px);
  font-weight: 900;
  letter-spacing: -0.05em;
  color: var(--gold);
  opacity: 0.75;
  line-height: 1;
  margin-bottom: 24px;
  display: block;
}
.pillar-title {
  font-size: clamp(26px, 2.4vw, 34px);
  font-weight: 700;
  letter-spacing: -0.04em;
  color: var(--navy);
  margin-bottom: 16px;
  line-height: 1.05;
}
.pillar-body {
  font-size: 15px;
  line-height: 1.75;
  color: var(--text-dim);
  max-width: 36ch;
}

@media (max-width: 680px) {
  .pillars-inner { grid-template-columns: 1fr; }
  .pillar { border-right: none; border-bottom: 1px solid var(--line); padding: 56px 20px; }
  .pillar:last-child { border-bottom: none; }
}
```

- [ ] **Step 2: Visual verify**

Open `index.html`. The services section should have a very light blue-tinted background. "01" and "02" should appear as large gold serif numbers (~96px), acting as visual anchors above the titles.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "design: pillars section — large gold numbers, indigo-bg tint"
```

---

### Task 3: How I Work — Dark Section Redesign

**Files:**
- Modify: `index.html` — CSS block for `.how-section` (lines ~609–643) and HTML section (lines ~1088–1101)

Full redesign: dark navy background, remove two-column grid, centered vertical numbered steps.

- [ ] **Step 1: Replace How I Work CSS**

Find and replace the entire How I Work CSS block:
```css
/* ── HOW I WORK ── */
.how-section {
  background: var(--bg);
  border-bottom: 1px solid var(--line);
}
.how-inner {
  max-width: var(--maxw);
  margin: 0 auto;
  padding: 96px 48px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 88px;
  align-items: start;
}
.how-label-col { padding-top: 8px; }
.how-title {
  font-size: clamp(36px, 4.5vw, 56px);
  font-weight: 700;
  letter-spacing: -0.045em;
  color: var(--navy);
  line-height: 1.0;
  margin-top: 18px;
}
.how-body p {
  font-size: 15.5px;
  line-height: 1.82;
  color: var(--text-body);
  margin-bottom: 20px;
  max-width: 50ch;
}
.how-body p:last-child { margin-bottom: 0; }

@media (max-width: 720px) {
  .how-inner { grid-template-columns: 1fr; gap: 32px; padding: 64px 20px; }
}
```

With:
```css
/* ── HOW I WORK ── */
.how-section {
  background: var(--dark-bg);
}
.how-inner {
  max-width: 700px;
  margin: 0 auto;
  padding: 104px 48px;
  text-align: center;
}
.how-title {
  font-size: clamp(36px, 4.5vw, 56px);
  font-weight: 700;
  font-style: italic;
  letter-spacing: -0.04em;
  color: oklch(97% 0.004 265);
  line-height: 1.0;
  margin-bottom: 72px;
}
.how-steps {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 44px;
  text-align: left;
}
.how-steps li {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.how-step-num {
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--gold);
}
.how-steps li p {
  font-size: 17px;
  line-height: 1.72;
  color: oklch(72% 0.012 265);
  max-width: 52ch;
}

@media (max-width: 720px) {
  .how-inner { padding: 72px 20px; }
  .how-title { margin-bottom: 52px; }
}
```

- [ ] **Step 2: Replace How I Work HTML**

Find this HTML block:
```html
<!-- HOW I WORK -->
<section class="how-section" id="how">
  <div class="how-inner">
    <div class="how-label-col reveal">
      <span class="t-label" style="display:block;margin-bottom:18px;">Approach</span>
      <h2 class="how-title">How I<br>work</h2>
    </div>
    <div class="how-body reveal" style="transition-delay:0.12s">
      <p>I work with a small number of clients at a time. Deeply, not broadly.</p>
      <p>No fluff, no shelfware. I move fast, get to the essential quickly, and leave you with something you can actually use.</p>
      <p>Most clients come through referral.</p>
    </div>
  </div>
</section>
```

Replace with:
```html
<!-- HOW I WORK -->
<section class="how-section" id="how">
  <div class="how-inner">
    <h2 class="how-title reveal">How I work</h2>
    <ol class="how-steps reveal" style="transition-delay:0.14s">
      <li>
        <span class="how-step-num">01</span>
        <p>I work with a small number of clients at a time. Deeply, not broadly.</p>
      </li>
      <li>
        <span class="how-step-num">02</span>
        <p>No fluff, no shelfware. I move fast, get to the essential quickly, and leave you with something you can actually use.</p>
      </li>
      <li>
        <span class="how-step-num">03</span>
        <p>Most clients come through referral.</p>
      </li>
    </ol>
  </div>
</section>
```

- [ ] **Step 3: Visual verify**

Open `index.html`. The How I Work section should be dark navy. Title "How I work" in large italic white serif, centered. Below it: three numbered statements (01/02/03 in gold, text in soft white), left-aligned, stacked vertically with generous spacing.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "design: how i work — dark navy section, vertical numbered steps, remove two-column grid"
```

---

### Task 4: Gold Section Divider

**Files:**
- Modify: `index.html` — CSS for `.clients-section` border-top (lines ~546–549)

Add one deliberate gold border between the Services section and the Clients section, as a visual accent that marks the transition.

- [ ] **Step 1: Add gold top border to clients section**

Find:
```css
/* ── CLIENTS ── */
.clients-section {
  background: var(--bg-alt);
  border-bottom: 1px solid var(--line);
}
```

Replace with:
```css
/* ── CLIENTS ── */
.clients-section {
  background: var(--bg-alt);
  border-top: 1px solid oklch(72% 0.09 75 / 0.35);
  border-bottom: 1px solid var(--line);
}
```

- [ ] **Step 2: Visual verify**

Scroll past the Services/Pillars section. There should be a subtle warm gold line marking the boundary before the Clients section. It should feel like punctuation, not decoration.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "design: add gold section divider between services and clients"
```

---

### Task 5: Final Visual QA

- [ ] **Step 1: Full scroll-through on desktop**

Open `index.html`. Scroll from top to bottom and verify:
- Crystal animates cleanly with no white box
- Hero (light) → Statement strip (dark indigo) → Services (light blue tint) — feels like alternating rhythm
- Pillars: large gold "01" / "02" numbers visible and dominant
- Clients section: subtle gold border at top
- How I Work: dark navy, italic centered title, three numbered statements
- Deliverables, About, Contact: unchanged

- [ ] **Step 2: Mobile check (DevTools 390px width)**

- Hero: crystal hidden (correct, display:none at ≤720px)
- Pillars stack to 1 column
- How I Work: full-width centered, title and steps readable
- No horizontal overflow

- [ ] **Step 3: Commit if clean**

```bash
git add index.html
git commit -m "design: section rhythm redesign — crystal fix, pillars, how i work dark section"
```
