# Noises Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a static, single-scroll marketing landing page for the Noises app that deploys directly to GitHub Pages.

**Architecture:** Three static files at the repo root — `index.html` (semantic markup + inline decorative SVG), `styles.css` (all styling via CSS variables + flexbox/grid, responsive, `prefers-reduced-motion`), and `main.js` (IntersectionObserver scroll-reveal + smooth anchor scroll). No build step, no frameworks, no runtime dependencies except a Google Fonts `<link>`.

**Tech Stack:** HTML5, CSS3, vanilla JS (ES6), inline SVG, Google Fonts. Deploys to GitHub Pages from repo root.

---

## Verification approach (read first)

This is a static presentational page with no application logic, and the repo has
no test framework. Classic unit-test TDD does not fit a marketing page — the
meaningful checks are **structural** (correct markup/landmarks present) and
**visual** (renders correctly, responsive, animates once). So each task verifies
via:

- **Structural checks** using `grep`/reading the file to confirm required
  markup exists (single `<h1>`, landmarks, all six feature cards, the CTA badge,
  no live store links, `prefers-reduced-motion` block, etc.).
- **Visual checks** by opening `index.html` in the Playwright browser and taking
  a screenshot / snapshot at desktop and mobile widths.

No mocking or unit-test harness is introduced (YAGNI).

---

## File Structure

```
index.html   # nav, hero (+ inline hills/stars SVG), features grid, closing CTA, footer
styles.css   # :root palette vars, base, nav, hero, features, cta, footer, responsive, reduced-motion
main.js      # scroll-reveal via IntersectionObserver + smooth in-page anchor scrolling
```

---

## Task 1: HTML skeleton + document head

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create `index.html` with head, fonts, and empty semantic landmarks**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Noises — Noise that never repeats</title>
  <meta name="description" content="Endless colored noise, synthesized live. Focus, relax, or fall asleep. Coming soon to the App Store." />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,600;12..96,800&family=Manrope:wght@400;500;600&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <header class="nav" id="top"></header>
  <main>
    <section class="hero" aria-labelledby="hero-title"></section>
    <section class="features" id="features" aria-labelledby="features-title"></section>
    <section class="cta" aria-labelledby="cta-title"></section>
  </main>
  <footer class="footer"></footer>
  <script src="main.js" defer></script>
</body>
</html>
```

- [ ] **Step 2: Verify structure**

Run: `grep -c -E "<(header|main|section|footer)" index.html`
Expected: at least `6` (header, main, 3 sections, footer).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add landing page HTML skeleton and head"
```

---

## Task 2: Top nav content

**Files:**
- Modify: `index.html` (fill `<header class="nav">`)

- [ ] **Step 1: Fill the nav**

Replace `<header class="nav" id="top"></header>` with:

```html
<header class="nav" id="top">
  <a class="wordmark" href="#top">Noises</a>
  <nav class="nav-links" aria-label="Primary">
    <a href="#features">Features</a>
    <span class="pill" aria-label="Coming soon">Coming soon</span>
  </nav>
</header>
```

- [ ] **Step 2: Verify**

Run: `grep -E "wordmark|nav-links|#features" index.html`
Expected: all three present. The "Coming soon" pill is a `<span>` (not a link) — confirm no `href` to a store.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add top nav (wordmark, features link, coming-soon pill)"
```

---

## Task 3: Hero content + inline SVG illustration

**Files:**
- Modify: `index.html` (fill `<section class="hero">`)

- [ ] **Step 1: Fill the hero with copy, badge, and the hills+stars SVG**

Replace `<section class="hero" aria-labelledby="hero-title"></section>` with:

```html
<section class="hero" aria-labelledby="hero-title">
  <div class="hero-inner">
    <h1 id="hero-title" class="reveal">Noise that never repeats.</h1>
    <p class="hero-sub reveal">Endless colored noise, synthesized live. Focus, relax, or fall asleep.</p>
    <span class="badge reveal" aria-label="Coming soon to the App Store">
      <svg width="16" height="16" viewBox="0 0 24 24" aria-hidden="true" fill="currentColor"><path d="M16.5 3c.1 1.2-.4 2.4-1.1 3.2-.8.9-2 1.6-3.2 1.5-.1-1.2.5-2.4 1.2-3.2.8-.9 2.1-1.5 3.1-1.5zM19 17.3c-.5 1.2-.8 1.7-1.5 2.7-1 1.5-2.4 3.3-4.1 3.3-1.5 0-1.9-1-4-1-2 0-2.5 1-4 1-1.7 0-3-1.6-4-3.1-2.8-4.2-3.1-9.1-1.4-11.7 1.2-1.9 3.1-3 4.9-3 1.8 0 2.9 1 4.4 1 1.4 0 2.3-1 4.4-1 1.6 0 3.3.9 4.5 2.4-4 2.2-3.3 7.9.3 9.4z"/></svg>
      Coming soon to the App Store
    </span>
  </div>
  <svg class="hero-art" viewBox="0 0 1440 320" preserveAspectRatio="xMidYMax slice" aria-hidden="true">
    <defs>
      <linearGradient id="hill-back" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0" stop-color="#5847b5"/><stop offset="1" stop-color="#2a2060"/>
      </linearGradient>
      <linearGradient id="hill-front" x1="0" y1="0" x2="0" y2="1">
        <stop offset="0" stop-color="#7d5cff"/><stop offset="1" stop-color="#43308f"/>
      </linearGradient>
    </defs>
    <g class="stars">
      <circle cx="180" cy="70" r="1.6"/><circle cx="420" cy="42" r="1.2"/>
      <circle cx="700" cy="80" r="1.8"/><circle cx="960" cy="50" r="1.2"/>
      <circle cx="1180" cy="72" r="1.6"/><circle cx="1320" cy="40" r="1.2"/>
      <circle cx="90" cy="120" r="1.2"/><circle cx="560" cy="110" r="1.2"/>
      <circle cx="1080" cy="120" r="1.4"/>
    </g>
    <path class="hill hill-back" d="M0 210 C300 140 560 240 820 190 C1080 140 1280 210 1440 185 L1440 320 L0 320 Z" fill="url(#hill-back)" opacity=".65"/>
    <path class="hill hill-front" d="M0 255 C360 200 640 285 940 235 C1160 200 1300 250 1440 240 L1440 320 L0 320 Z" fill="url(#hill-front)"/>
  </svg>
</section>
```

- [ ] **Step 2: Verify single h1, badge is not a store link, SVG present**

Run: `grep -c "<h1" index.html`  → Expected: `1`
Run: `grep -E "hero-art|hill-front|Coming soon to the App Store" index.html` → Expected: all present.
Confirm the badge is a `<span>` with no `href` (no dead store link).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add hero copy, coming-soon badge, and hills/stars SVG"
```

---

## Task 4: Features grid (six cards)

**Files:**
- Modify: `index.html` (fill `<section class="features">`)

- [ ] **Step 1: Fill the features section with a heading and six cards**

Replace `<section class="features" id="features" aria-labelledby="features-title"></section>` with:

```html
<section class="features" id="features" aria-labelledby="features-title">
  <h2 id="features-title" class="section-title reveal">Made for focus, calm, and sleep</h2>
  <ul class="feature-grid">
    <li class="feature reveal">
      <div class="swatches" aria-hidden="true">
        <span style="background:#d7d7dd"></span><span style="background:#ff6fa5"></span>
        <span style="background:#a06a3f"></span><span style="background:#e2564a"></span>
        <span style="background:#8a4dff"></span><span style="background:#24c46e"></span>
        <span style="background:#8b8e95"></span>
      </div>
      <h3>Seven noise colors</h3>
      <p>White, Pink, Brown, Red, Violet, Green, and Gray — each with its own character.</p>
    </li>
    <li class="feature reveal">
      <h3>Endless &amp; non-repeating</h3>
      <p>Synthesized live by an in-app audio graph, so it never loops or repeats.</p>
    </li>
    <li class="feature reveal">
      <h3>Tune the sound</h3>
      <p>A 2D pad shapes each color's timbre — soft ↔ bright and deep ↔ clear, in real time.</p>
    </li>
    <li class="feature reveal">
      <h3>Countdown timer</h3>
      <p>Set hours and minutes; playback stops automatically when the timer ends.</p>
    </li>
    <li class="feature reveal">
      <h3>Remembers your setup</h3>
      <p>Your last color and timer are saved on device and restored next launch.</p>
    </li>
    <li class="feature reveal">
      <h3>AirPlay on iOS</h3>
      <p>Route playback to HomePod, Apple TV, or AirPlay speakers.</p>
    </li>
  </ul>
</section>
```

- [ ] **Step 2: Verify six feature cards exist**

Run: `grep -c 'class="feature reveal"' index.html`
Expected: `6`

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add features grid with six feature cards"
```

---

## Task 5: Closing CTA + footer

**Files:**
- Modify: `index.html` (fill `<section class="cta">` and `<footer>`)

- [ ] **Step 1: Fill the closing CTA**

Replace `<section class="cta" aria-labelledby="cta-title"></section>` with:

```html
<section class="cta" aria-labelledby="cta-title">
  <h2 id="cta-title" class="section-title reveal">Quiet is coming.</h2>
  <p class="cta-sub reveal">Noises is launching soon on iOS and Android.</p>
  <span class="badge reveal" aria-label="Coming soon to the App Store">
    <svg width="16" height="16" viewBox="0 0 24 24" aria-hidden="true" fill="currentColor"><path d="M16.5 3c.1 1.2-.4 2.4-1.1 3.2-.8.9-2 1.6-3.2 1.5-.1-1.2.5-2.4 1.2-3.2.8-.9 2.1-1.5 3.1-1.5zM19 17.3c-.5 1.2-.8 1.7-1.5 2.7-1 1.5-2.4 3.3-4.1 3.3-1.5 0-1.9-1-4-1-2 0-2.5 1-4 1-1.7 0-3-1.6-4-3.1-2.8-4.2-3.1-9.1-1.4-11.7 1.2-1.9 3.1-3 4.9-3 1.8 0 2.9 1 4.4 1 1.4 0 2.3-1 4.4-1 1.6 0 3.3.9 4.5 2.4-4 2.2-3.3 7.9.3 9.4z"/></svg>
    Coming soon to the App Store
  </span>
</section>
```

- [ ] **Step 2: Fill the footer**

Replace `<footer class="footer"></footer>` with:

```html
<footer class="footer">
  <p>Noises © 2026</p>
  <a href="https://github.com/noises-app/noises-app.github.io" target="_blank" rel="noopener">View on GitHub</a>
</footer>
```

Note: if the exact GitHub repo URL differs, adjust the `href`; the placeholder points at the pages repo owner.

- [ ] **Step 3: Verify**

Run: `grep -E "Noises © 2026|View on GitHub|class=\"cta\"" index.html`
Expected: all present.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add closing CTA and footer"
```

---

## Task 6: Base styles + palette + typography

**Files:**
- Create: `styles.css`

- [ ] **Step 1: Create `styles.css` with reset, variables, base type, and background wash**

```css
:root {
  --bg: #0b0e26;
  --bg-2: #08091a;
  --violet: #8a4dff;
  --pink: #ff6fa5;
  --text: #f4f5fb;
  --muted: #a7abc4;
  --maxw: 1100px;
  --font-display: "Bricolage Grotesque", system-ui, sans-serif;
  --font-body: "Manrope", system-ui, sans-serif;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  font-family: var(--font-body);
  color: var(--text);
  background:
    radial-gradient(120% 70% at 50% -10%, rgba(138,77,255,.38), transparent 55%),
    radial-gradient(90% 60% at 85% 5%, rgba(255,111,165,.18), transparent 55%),
    var(--bg);
  min-height: 100vh;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}
h1, h2, h3 { font-family: var(--font-display); letter-spacing: -0.02em; line-height: 1.05; }
a { color: inherit; }
img, svg { display: block; }
```

- [ ] **Step 2: Verify file is linked and parses**

Run: `grep -E "\-\-violet|font-display|radial-gradient" styles.css`
Expected: all present.

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "feat: add base styles, palette variables, and background wash"
```

---

## Task 7: Nav, hero, and badge styles

**Files:**
- Modify: `styles.css` (append)

- [ ] **Step 1: Append nav + hero + badge styles**

```css
/* Nav */
.nav {
  display: flex; align-items: center; justify-content: space-between;
  max-width: var(--maxw); margin: 0 auto; padding: 22px 24px;
}
.wordmark { font-family: var(--font-display); font-weight: 800; font-size: 22px; text-decoration: none; }
.nav-links { display: flex; align-items: center; gap: 20px; }
.nav-links a { text-decoration: none; font-weight: 500; font-size: 15px; opacity: .8; }
.nav-links a:hover { opacity: 1; }
.pill {
  font-size: 12px; font-weight: 600; padding: 6px 12px; border-radius: 999px;
  border: 1px solid rgba(255,255,255,.25); color: var(--text);
}

/* Hero */
.hero { position: relative; overflow: hidden; min-height: 82vh; display: flex; flex-direction: column; }
.hero-inner {
  max-width: var(--maxw); margin: 0 auto; padding: 8vh 24px 0; text-align: center;
  position: relative; z-index: 2;
}
.hero h1 { font-size: clamp(38px, 7vw, 76px); font-weight: 800; }
.hero-sub { margin: 18px auto 30px; max-width: 560px; font-size: clamp(16px, 2.2vw, 20px); color: var(--muted); }

/* Badge (shared) */
.badge {
  display: inline-flex; align-items: center; gap: 9px;
  background: #fff; color: #111; font-weight: 600; font-size: 14px;
  padding: 12px 18px; border-radius: 12px;
  box-shadow: 0 10px 30px rgba(138,77,255,.25);
}

/* Hero illustration */
.hero-art { position: absolute; bottom: 0; left: 0; width: 100%; height: 46vh; min-height: 220px; z-index: 1; }
.hero-art .stars circle { fill: #fff; opacity: .75; }
```

- [ ] **Step 2: Verify**

Run: `grep -E "\.hero-art|\.badge|\.wordmark" styles.css`
Expected: all present.

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "feat: style nav, hero, badge, and hero illustration"
```

---

## Task 8: Features, CTA, and footer styles

**Files:**
- Modify: `styles.css` (append)

- [ ] **Step 1: Append section styles**

```css
/* Shared section title */
.section-title { text-align: center; font-size: clamp(26px, 4vw, 40px); font-weight: 800; }

/* Features */
.features { max-width: var(--maxw); margin: 0 auto; padding: 90px 24px; }
.features .section-title { margin-bottom: 42px; }
.feature-grid { list-style: none; display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; }
.feature {
  background: rgba(255,255,255,.04); border: 1px solid rgba(255,255,255,.08);
  border-radius: 18px; padding: 26px;
}
.feature h3 { font-size: 19px; margin-bottom: 8px; }
.feature p { color: var(--muted); font-size: 15px; }
.swatches { display: flex; gap: 7px; margin-bottom: 16px; }
.swatches span { width: 20px; height: 20px; border-radius: 50%; box-shadow: 0 0 0 1px rgba(255,255,255,.15); }

/* Closing CTA */
.cta { text-align: center; padding: 90px 24px 100px; }
.cta-sub { color: var(--muted); margin: 14px auto 28px; max-width: 460px; }

/* Footer */
.footer {
  max-width: var(--maxw); margin: 0 auto; padding: 30px 24px 50px;
  display: flex; align-items: center; justify-content: space-between;
  border-top: 1px solid rgba(255,255,255,.08); color: var(--muted); font-size: 14px;
}
.footer a { text-decoration: none; opacity: .85; }
.footer a:hover { opacity: 1; }
```

- [ ] **Step 2: Verify**

Run: `grep -E "\.feature-grid|\.cta|\.footer" styles.css`
Expected: all present.

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "feat: style features grid, closing CTA, and footer"
```

---

## Task 9: Responsive rules + reduced-motion + reveal animation styles

**Files:**
- Modify: `styles.css` (append)

- [ ] **Step 1: Append reveal states, responsive breakpoints, and reduced-motion**

```css
/* Scroll-reveal (JS adds .is-visible) */
.reveal { opacity: 0; transform: translateY(16px); transition: opacity .7s ease, transform .7s ease; }
.reveal.is-visible { opacity: 1; transform: none; }
/* Staggered hero load */
.hero .reveal:nth-child(1) { transition-delay: .05s; }
.hero .reveal:nth-child(2) { transition-delay: .15s; }
.hero .reveal:nth-child(3) { transition-delay: .25s; }

/* Tablet */
@media (max-width: 860px) {
  .feature-grid { grid-template-columns: repeat(2, 1fr); }
}
/* Mobile */
@media (max-width: 560px) {
  .feature-grid { grid-template-columns: 1fr; }
  .nav { padding: 18px 18px; }
  .nav-links { gap: 12px; }
  .footer { flex-direction: column; gap: 10px; text-align: center; }
}

/* Respect reduced motion */
@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
  .reveal { opacity: 1; transform: none; transition: none; }
}
```

- [ ] **Step 2: Verify**

Run: `grep -E "prefers-reduced-motion|max-width: 560px|\.reveal" styles.css`
Expected: all present.

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "feat: add responsive rules, reveal animation, and reduced-motion support"
```

---

## Task 10: Scroll-reveal JS + smooth anchor scroll

**Files:**
- Create: `main.js`

- [ ] **Step 1: Create `main.js`**

```js
// Reveal elements as they scroll into view; respect reduced-motion.
(function () {
  var reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
  var revealables = document.querySelectorAll(".reveal");

  if (reduce || !("IntersectionObserver" in window)) {
    revealables.forEach(function (el) { el.classList.add("is-visible"); });
    return;
  }

  var io = new IntersectionObserver(function (entries) {
    entries.forEach(function (entry) {
      if (entry.isIntersecting) {
        entry.target.classList.add("is-visible");
        io.unobserve(entry.target);
      }
    });
  }, { threshold: 0.12 });

  revealables.forEach(function (el) { io.observe(el); });
})();
```

Note: smooth anchor scrolling is handled by CSS `scroll-behavior: smooth` (Task 6), disabled under reduced-motion (Task 9), so no JS is needed for it (YAGNI).

- [ ] **Step 2: Verify**

Run: `grep -E "IntersectionObserver|prefers-reduced-motion|is-visible" main.js`
Expected: all present.

- [ ] **Step 3: Commit**

```bash
git add main.js
git commit -m "feat: add scroll-reveal via IntersectionObserver"
```

---

## Task 11: Visual + structural verification pass

**Files:** none (verification only)

- [ ] **Step 1: Structural sanity checks**

Run and confirm each:
- `grep -c "<h1" index.html` → `1` (exactly one h1)
- `grep -c 'class="feature reveal"' index.html` → `6`
- `grep -c "Coming soon to the App Store" index.html` → `2` (hero + closing CTA)
- `grep -Ei "apps.apple.com|play.google.com|itunes.apple.com" index.html` → no matches (no live store links)

- [ ] **Step 2: Desktop render**

Open the file in the Playwright browser (`file://<abs path>/index.html`), resize to 1280×800, take a full-page screenshot. Confirm: nav renders, headline + subhead + badge centered, hills/stars SVG spans full width at the hero bottom, six feature cards in a 3-column grid, closing CTA + footer present.

- [ ] **Step 3: Mobile render**

Resize to 390×844, take a full-page screenshot. Confirm: no horizontal overflow, feature grid collapses to one column, footer stacks, headline scales down.

- [ ] **Step 4: Reduced-motion check**

In the browser, emulate `prefers-reduced-motion: reduce`, reload, confirm all `.reveal` content is visible immediately (no hidden/blank sections).

- [ ] **Step 5: Fix any issues found, then final commit if changes were made**

```bash
git add -A
git commit -m "fix: address issues found during visual verification"
```

---

## Self-Review (completed by plan author)

**Spec coverage:**
- Goal / coming-soon CTA → Tasks 3, 5 (badge, no store links; verified Task 11).
- Structure Hero→Features→CTA→Footer → Tasks 3, 4, 5.
- Deep indigo + radial wash, violet/pink palette → Task 6.
- Hills+stars inline SVG at hero bottom → Task 3 + Task 7 styling.
- Distinctive display font + body font (Google Fonts) → Task 1 link + Task 6 vars.
- Six features (7 colors, endless, tune, timer, remembers, AirPlay) → Task 4.
- Static, no build, 3 files → Tasks 1–10.
- Responsive → Task 9. Accessibility (landmarks, single h1, aria-hidden SVG,
  focus/contrast) → Tasks 1–5 markup + Task 11 checks. Reduced-motion → Tasks 9, 10.
- Footer "Noises © 2026" + GitHub link → Task 5.

**Placeholder scan:** No TBD/TODO. Only note: GitHub URL in Task 5 may need the
real repo owner — flagged inline for the implementer to confirm.

**Type/name consistency:** Class names (`.reveal`/`.is-visible`, `.badge`,
`.feature reveal`, `.hero-art`, `.section-title`, `.feature-grid`) are used
consistently across HTML (Tasks 2–5), CSS (Tasks 6–9), and JS (Task 10).
