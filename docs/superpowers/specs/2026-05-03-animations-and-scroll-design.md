# Design Spec — Animations, Scroll Journey Fix & Mobile Swiping

**Date:** 2026-05-03  
**Status:** Approved

---

## Overview

Three improvements to `index.html`:

1. Fix the scroll journey line so section labels appear when the pin reaches that section's heading — not based on rough viewport thresholds
2. Add scroll-triggered animations (fade up for headings, staggered fade up for cards) across all sections on desktop and mobile
3. Add horizontal swipe card carousels on mobile for the Services and Work sections

Plus a `CLAUDE.md` update to capture these decisions for future sessions.

---

## Feature 1 — Scroll Journey Line Fix

**Problem:** Labels currently fire at hardcoded scroll thresholds that drift from where sections actually sit.

**Solution:**
- On `DOMContentLoaded` and on `window resize`, measure the real `offsetTop` of each section heading using `getBoundingClientRect().top + window.scrollY`
- Build an array of `{ y, label }` objects: About, Services, Work, Approach, Experience
- In the existing scroll handler, the pin's progress is already expressed as a ratio (0–1) of total scroll distance. Convert each section Y into the same ratio: `sectionY / (document.body.scrollHeight - window.innerHeight)`
- When the pin's ratio crosses a section's ratio, swap the label. Fade the label in/out with a 200ms CSS opacity transition
- Recalculate section positions on resize (layout reflows change heading positions)

**Sections with labels:** About · Services · Work · Approach · Experience  
**Desktop only:** The journey line is already hidden below 900px — no mobile changes needed here

---

## Feature 2 — Scroll-Triggered Animations

**Animation style (confirmed in demo v4):**
- Headings: single smooth fade up as one block
- Cards and boxes: staggered fade up — each card cascades in after the previous
- Labels and body text: fade up with short delay after heading

**Timing:**
- Duration: 750ms (headings), 750ms (everything else)
- Easing: `cubic-bezier(0.16, 1, 0.3, 1)` — fast ease-out, smooth land
- Travel distance: `translateY(44px)` → `translateY(0)`
- Stagger delays: label 0s · heading 0.1s · body 0.22s · card 1 0.22s · card 2 0.34s · card 3 0.48s

**Implementation:**
- Add `.anim` class to every element that should animate, with `.d1`–`.d4` delay variants on staggered items
- A single `IntersectionObserver` (threshold 0.1) adds `.visible` when elements enter the viewport
- CSS handles the rest: `.anim { opacity: 0; transform: translateY(44px); transition: ... }` and `.anim.visible { opacity: 1; transform: translateY(0) }`

**Scope — sections that get animations:**
- Hero: excluded (visible on load, no entry animation needed)
- About (dark): label, heading, pillars/cards
- Services (cream): label, heading, body, 3 service cards
- Work / Portfolio (cream): label, heading, all project cards
- Approach (black): label, heading, intro text, 4 step cards
- Trusted By (cream): label, heading, 4 client logo cards
- Contact (dark): label, heading, form fields

**Accessibility:**
```css
@media (prefers-reduced-motion: reduce) {
  .anim { transition-duration: 0ms; }
}
```

---

## Feature 3 — Mobile Card Swiping

**Applies to:** Services section (3 cards) and Work/Portfolio section (all project cards)  
**Breakpoint:** `max-width: 767px`

**Behaviour:**
- Cards switch from a wrapped grid to a horizontal scroll row
- Each card snaps to the left edge as the user swipes
- The next card peeks ~20px into view to signal scrollability
- Scrollbar is hidden visually but scrolling still works

**CSS pattern:**
```css
@media (max-width: 767px) {
  .cards-container {
    display: flex;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;
    gap: 16px;
    padding-bottom: 8px;
  }
  .cards-container::-webkit-scrollbar { display: none; }

  .card {
    scroll-snap-align: start;
    min-width: 80vw;
    flex-shrink: 0;
  }
}
```

No JavaScript required. The featured full-width Work card is excluded from swiping (it's already full-width on mobile).

---

## CLAUDE.md Update

Add a new section to `CLAUDE.md` capturing these decisions so future sessions don't re-litigate them:

- **Animation style:** fade up for headings (single block), staggered fade up for cards. Duration 750ms, easing `cubic-bezier(0.16, 1, 0.3, 1)`, travel `44px`. Always include `prefers-reduced-motion` override.
- **Scroll journey line:** pin-position driven (not viewport driven). Section Y positions must be measured on load and remeasured on resize. Labels fade with 200ms opacity transition.
- **Mobile card pattern:** CSS scroll-snap horizontal scroll, `min-width: 80vw` per card, scrollbar hidden, applied at `max-width: 767px`.

---

## Out of Scope

- Hero section animations (it's the first visible section — nothing to animate into)
- Nav or footer animations
- Any changes to the journey line SVG path shape or pin design
- JS-based touch carousels or third-party libraries
