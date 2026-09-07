# Noises Landing Page — Design

**Date:** 2026-09-07
**Repo:** `noises-app.github.io` (GitHub Pages static site)
**App:** Noises — a colored-noise player for iOS/Android (Expo/React Native)

## Goal

A single-scroll marketing landing page that promotes the Noises app. The app is
pre-launch, so the primary call-to-action is a **"Coming soon to the App Store"**
badge (no live store link, no email capture, no backend).

## Audience & Tone

Prospective users who want a focus/relax/sleep sound tool. Tone: calm, premium,
atmospheric — matching the app's dark, immersive UI.

## Visual Direction

Inspired by the "Sleepiest" reference the user provided: a deep indigo hero with
a centered headline, muted subhead, a single centered CTA badge, a simple top nav
(wordmark left, links right), and a layered illustration anchored to the bottom.

- **Background:** deep indigo `#0b0e26` (matches the app splash `backgroundColor`),
  with a soft radial color wash bleeding from the top-center in the app's
  violet/indigo family.
- **Hero illustration (bottom):** **rolling hills + stars** rendered as pure inline
  SVG (Option C) — gentle layered hills in violet/indigo gradients under a faint
  starfield. No image files; scales crisply at any width. Subtle entrance animation.
- **Palette:** indigo base `#0b0e26`; violet accent `#8a4dff`; pink accent `#ff6fa5`
  (the app's default color). Text white `#f4f5fb`, muted `#aab`.
- **Type:** distinctive display font (Bricolage Grotesque or Fraunces) for the
  headline, paired with a clean body sans. Loaded from Google Fonts.
- **Motion:** one orchestrated page-load reveal (staggered fade/rise of nav →
  headline → subhead → badge → illustration). Keep it tasteful and calm.

## Page Structure (single scroll)

Standard structure: **Hero → Features → CTA → Footer**

1. **Top nav** — "Noises" wordmark (left); "Features" anchor link + a small
   "Coming soon" pill (right). Collapses gracefully on mobile.
2. **Hero** — full-viewport section:
   - Headline: *"Noise that never repeats."*
   - Subhead: *"Endless colored noise, synthesized live. Focus, relax, or fall asleep."*
   - CTA: **"Coming soon to the App Store"** badge (non-interactive / not a live link).
   - Bottom: the layered hills-and-stars SVG illustration.
3. **Features** — a responsive grid of the app's key strengths, each with a short
   title + one-line description and a small icon/visual:
   - **Seven noise colors** — White, Pink, Brown, Red, Violet, Green, Gray.
   - **Endless, non-repeating** — synthesized live, never loops.
   - **Tune the sound** — a 2D pad shapes each color's timbre (soft↔bright, deep↔clear).
   - **Countdown timer** — set hours/minutes; playback stops automatically.
   - **Remembers your setup** — last color + timer restored on next launch.
   - **AirPlay (iOS)** — route playback to HomePod, Apple TV, or AirPlay speakers.
4. **Closing CTA** — a centered restatement: short line + the "Coming soon" badge.
5. **Footer** — "Noises © 2026" and a link to the GitHub repository.

## Technical Approach

- **Static site, no build step.** A single `index.html` with a linked
  `styles.css` and a small `main.js` (for scroll-reveal / anchor scrolling only).
  Deploys directly to GitHub Pages from the repo root.
- **No frameworks, no dependencies** beyond a Google Fonts `<link>`. All
  illustration is inline SVG; all layout is CSS (flexbox/grid).
- **Responsive:** mobile-first; nav, hero type, and the features grid adapt down
  to small screens. Illustration uses `preserveAspectRatio` to span full width.
- **Accessibility:** semantic landmarks (`header`/`main`/`section`/`footer`),
  a single `h1`, sufficient color contrast for text on the dark wash, `alt`/
  `aria-hidden` on decorative SVG, visible focus states, and
  `prefers-reduced-motion` respected (disable/soften entrance animations).
- **Performance:** no images to download (SVG inline), one font request,
  minimal JS. Should be effectively instant.

## Files

```
index.html      # markup: nav, hero, features, CTA, footer + inline SVG
styles.css      # all styling, CSS variables for the palette, responsive rules
main.js         # scroll-reveal (IntersectionObserver) + smooth anchor scroll
```

## Out of Scope (YAGNI)

- Email/waitlist capture and any backend or form handler.
- Live App Store / Google Play links (app not released).
- Blog, articles, search, multi-page routing.
- A color-showcase carousel and a "how it works" audio-graph explainer (that was
  the "Rich" structure option, not chosen).

## Success Criteria

- Opening `index.html` renders a polished, dark, calm landing page matching the
  agreed direction, with all sections present.
- The hills-and-stars SVG illustration renders correctly and spans the hero width.
- Layout is responsive from ~320px up to desktop with no overflow/broken wrapping.
- "Coming soon to the App Store" is the only CTA; no dead store links.
- Page is self-contained static assets deployable to GitHub Pages with no build.
- Entrance animation runs once on load and is disabled under
  `prefers-reduced-motion`.
