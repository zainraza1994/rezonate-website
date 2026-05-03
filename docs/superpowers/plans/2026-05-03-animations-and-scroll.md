# Animations, Scroll Journey Fix & Mobile Swiping — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Fix the scroll journey pin labels to fire at the correct position, add premium scroll-triggered animations across all sections, and enable horizontal card swiping on mobile for Services and Work sections.

**Architecture:** All changes are made directly to the single `index.html` file. No build tools, no external libraries, no new files. CSS changes go in the `<style>` block; JS changes go in the `<script>` block at the bottom; HTML changes add classes to existing elements.

**Tech Stack:** Vanilla HTML/CSS/JS. IntersectionObserver API for animations. CSS scroll-snap for mobile swiping. No npm, no frameworks.

---

## File Map

| File | What changes |
|---|---|
| `index.html:218-219` | Replace `.fade-in` CSS with new `.anim` system |
| `index.html:222-245` | Add new `@media (max-width: 767px)` block for card swiping after existing 900px block |
| `index.html:299-334` | Add `.anim` classes to About section elements |
| `index.html:337-381` | Add `.anim` classes to Services section elements |
| `index.html:384-560` | Replace `.fade-in` with `.anim` on portfolio cards; add `.anim` to header |
| `index.html:563-599` | Add `.anim` classes to Approach section elements |
| `index.html:602-630` | Add `.anim` classes to Trusted By section elements |
| `index.html:633-692` | Add `.anim` classes to Contact section elements |
| `index.html:717-723` | `sectionDefs` — no structural change, just gains `.ratio` at runtime |
| `index.html:746-780` | `initJourney()` — add section Y ratio calculation |
| `index.html:826-841` | `updateJourney()` — replace viewport-based label logic with ratio comparison |
| `index.html:852-860` | Replace `fade-in` observer with `anim` observer |
| `CLAUDE.md` | Add animation + scroll + mobile-swipe decisions section |

---

## Task 1: Fix scroll journey label accuracy

**File:** `index.html`

The current label fires when `scrollTop >= el.offsetTop - window.innerHeight * 0.7` — a viewport threshold, not pin-driven. Replace with ratio comparison so the label changes exactly when the pin reaches a section's heading.

- [ ] **Step 1: Add ratio calculation to `initJourney()`**

  Find the block ending with `contactStart = ...` (around line 776). Add these lines immediately after `contactStart` is assigned and before `journeyReady = true`:

  ```js
  const docH = document.documentElement.scrollHeight - window.innerHeight;
  sectionDefs.forEach(s => {
    const el = document.getElementById(s.id);
    if (!el) return;
    const heading = el.querySelector('.section-title') || el;
    const headingY = heading.getBoundingClientRect().top + window.scrollY;
    s.ratio = (headingY / docH) / contactStart;
  });
  ```

- [ ] **Step 2: Replace label logic in `updateJourney()`**

  Find this block in `updateJourney()` (around line 826):

  ```js
  let currentLabel = '';
  for (const s of sectionDefs) {
    const el = document.getElementById(s.id);
    if (el && scrollTop >= el.offsetTop - window.innerHeight * 0.7) {
      currentLabel = s.label;
    }
  }
  ```

  Replace it with:

  ```js
  let currentLabel = '';
  for (const s of sectionDefs) {
    if (s.ratio !== undefined && journeyPct >= s.ratio) {
      currentLabel = s.label;
    }
  }
  ```

- [ ] **Step 3: Verify in browser**

  Open `index.html` in Chrome. Scroll slowly through the page on desktop. As the red pin reaches each section's heading text, the label beside it should change — "About" when pin reaches the About heading, "Services" when it reaches the Services heading, etc. The label should NOT change before the pin is visually at that heading.

- [ ] **Step 4: Commit**

  ```bash
  git add index.html
  git commit -m "fix: scroll journey labels now fire when pin reaches section heading"
  ```

---

## Task 2: Replace animation CSS

**File:** `index.html` — the `<style>` block

The current `.fade-in` system (line 218–219) uses 24px travel and no stagger delays. Replace it with the approved `.anim` system: 44px travel, 750ms ease-out, delay variants for staggering.

- [ ] **Step 1: Replace the animation CSS block**

  Find and replace these exact lines:

  ```css
  /* ANIMATIONS */
  .fade-in { opacity: 0; transform: translateY(24px); transition: opacity 0.7s var(--ease), transform 0.7s var(--ease); }
  .fade-in.visible { opacity: 1; transform: translateY(0); }
  ```

  Replace with:

  ```css
  /* ANIMATIONS */
  .anim { opacity: 0; transform: translateY(44px); transition: opacity 0.75s var(--ease), transform 0.75s var(--ease); }
  .anim.d1 { transition-delay: 0.10s; }
  .anim.d2 { transition-delay: 0.22s; }
  .anim.d3 { transition-delay: 0.34s; }
  .anim.d4 { transition-delay: 0.48s; }
  .anim.visible { opacity: 1; transform: translateY(0); }
  @media (prefers-reduced-motion: reduce) { .anim { transition-duration: 0ms !important; transition-delay: 0ms !important; } }
  ```

- [ ] **Step 2: Replace the animation observer JS**

  Find and replace this block (around line 852):

  ```js
  // FADE IN
  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry, i) => {
      if (entry.isIntersecting) {
        setTimeout(() => entry.target.classList.add('visible'), i * 100);
        observer.unobserve(entry.target);
      }
    });
  }, { threshold: 0.08 });
  document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
  ```

  Replace with:

  ```js
  // SCROLL ANIMATIONS
  const animObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
        animObserver.unobserve(entry.target);
      }
    });
  }, { threshold: 0.1 });
  document.querySelectorAll('.anim').forEach(el => animObserver.observe(el));
  ```

- [ ] **Step 3: Commit**

  ```bash
  git add index.html
  git commit -m "feat: replace fade-in animation system with anim + stagger delays"
  ```

---

## Task 3: Add animation classes — About section

**File:** `index.html` — About section (~line 299)

- [ ] **Step 1: Add `.anim` classes to About elements**

  Find:
  ```html
  <div class="section-label">About Rezonate</div>
  <h2 class="section-title">People-centred design.<br>Real-world impact.</h2>
  <div class="about-grid">
  ```

  Replace with:
  ```html
  <div class="section-label anim">About Rezonate</div>
  <h2 class="section-title anim d1">People-centred design.<br>Real-world impact.</h2>
  <div class="about-grid anim d2">
  ```

- [ ] **Step 2: Verify in browser**

  Reload `index.html`. Scroll to the About section. The label, heading, and about grid should each fade up in sequence — label first, heading second, grid third.

- [ ] **Step 3: Commit**

  ```bash
  git add index.html
  git commit -m "feat: add scroll animations to About section"
  ```

---

## Task 4: Add animation classes — Services section

**File:** `index.html` — Services section (~line 337)

- [ ] **Step 1: Add `.anim` to the services header**

  Find:
  ```html
  <div class="services-header">
  ```
  Replace with:
  ```html
  <div class="services-header anim">
  ```

- [ ] **Step 2: Add staggered `.anim` to the three service cards**

  Find:
  ```html
  <div class="services-grid">
        <div class="service-card">
  ```
  The three service cards open with `<div class="service-card">`. Add delay classes so they cascade in. The first card gets `anim d1`, second `anim d2`, third `anim d3`:

  Replace the opening tag of each service card:
  - First `<div class="service-card">` → `<div class="service-card anim d1">`
  - Second `<div class="service-card">` → `<div class="service-card anim d2">`
  - Third `<div class="service-card">` → `<div class="service-card anim d3">`

- [ ] **Step 3: Verify in browser**

  Reload and scroll to Services. The header fades up first, then the three cards stagger in one after another left to right.

- [ ] **Step 4: Commit**

  ```bash
  git add index.html
  git commit -m "feat: add scroll animations to Services section"
  ```

---

## Task 5: Add animation classes — Work / Portfolio section

**File:** `index.html` — Portfolio section (~line 384)

The portfolio cards currently have `.fade-in` — remove those and replace with `.anim`. Add `.anim` to the header too.

- [ ] **Step 1: Animate the portfolio header**

  Find:
  ```html
  <div class="portfolio-header">
  ```
  Replace with:
  ```html
  <div class="portfolio-header anim">
  ```

- [ ] **Step 2: Replace `.fade-in` on the featured card**

  Find:
  ```html
  <div class="project-card featured fade-in">
  ```
  Replace with:
  ```html
  <div class="project-card featured anim">
  ```

- [ ] **Step 3: Replace `.fade-in` on the remaining 6 project cards with staggered `.anim`**

  The 6 non-featured cards appear in pairs (2-column grid on desktop). Add alternating delays so each pair staggers:

  - Card 2 (first after featured): `project-card anim`
  - Card 3: `project-card anim d1`
  - Card 4: `project-card anim`
  - Card 5: `project-card anim d1`
  - Card 6: `project-card anim`
  - Card 7: `project-card anim d1`

  Find each `<div class="project-card fade-in">` in order and replace:
  ```html
  <!-- Card 2 --> <div class="project-card fade-in"> → <div class="project-card anim">
  <!-- Card 3 --> <div class="project-card fade-in"> → <div class="project-card anim d1">
  <!-- Card 4 --> <div class="project-card fade-in"> → <div class="project-card anim">
  <!-- Card 5 --> <div class="project-card fade-in"> → <div class="project-card anim d1">
  <!-- Card 6 --> <div class="project-card fade-in"> → <div class="project-card anim">
  <!-- Card 7 --> <div class="project-card fade-in"> → <div class="project-card anim d1">
  ```

- [ ] **Step 4: Verify in browser**

  Reload and scroll to Work. Header fades first, featured card fades in, then pairs of cards stagger in as you scroll past them.

- [ ] **Step 5: Commit**

  ```bash
  git add index.html
  git commit -m "feat: add scroll animations to Work/Portfolio section"
  ```

---

## Task 6: Add animation classes — Approach, Trusted By, Contact sections

**File:** `index.html` — lines 563–692

- [ ] **Step 1: Approach section**

  Find:
  ```html
  <div class="approach-header">
  ```
  Replace with:
  ```html
  <div class="approach-header anim">
  ```

  Then find the four step cards. They open with `<div class="step">`. Add stagger classes:
  - Step 1: `<div class="step anim d1">`
  - Step 2: `<div class="step anim d2">`
  - Step 3: `<div class="step anim d3">`
  - Step 4: `<div class="step anim d4">`

- [ ] **Step 2: Trusted By section**

  Find:
  ```html
  <div class="section-label">Experience</div>
  <h2 class="section-title">Trusted by</h2>
  ```
  Replace with:
  ```html
  <div class="section-label anim">Experience</div>
  <h2 class="section-title anim d1">Trusted by</h2>
  ```

  Then find the `.logo-strip` div and add `anim d2` to it:
  ```html
  <div class="logo-strip anim d2">
  ```

- [ ] **Step 3: Contact section**

  Find:
  ```html
  <div class="section-label">Get in touch</div>
  <h2 class="section-title">Let's work<br>together.</h2>
  ```
  Replace with:
  ```html
  <div class="section-label anim">Get in touch</div>
  <h2 class="section-title anim d1">Let's work<br>together.</h2>
  ```

  Then find `.contact-sub` and add `anim d2`:
  ```html
  <p class="contact-sub anim d2">
  ```

  Then find `.contact-details` and `.contact-form` and add staggered anim:
  ```html
  <div class="contact-details anim d2">
  <div class="contact-form anim d3">
  ```

- [ ] **Step 4: Verify all three sections in browser**

  Reload and scroll through Approach, Trusted By, and Contact. Each section's label and heading should fade up on entry, with cards and content staggering in after.

- [ ] **Step 5: Commit**

  ```bash
  git add index.html
  git commit -m "feat: add scroll animations to Approach, Trusted By, and Contact sections"
  ```

---

## Task 7: Mobile card swiping — Services + Portfolio

**File:** `index.html` — the `<style>` block, after the existing `@media (max-width: 900px)` block

At mobile widths, both grids should become horizontal scroll carousels. The 900px block already stacks them vertically (`grid-template-columns: 1fr`). Add a new 767px block after it to override those into flex scroll containers.

- [ ] **Step 1: Add the mobile swipe CSS block**

  Find the closing `}` of the existing `@media (max-width: 900px)` block (around line 245). Add this new block immediately after it:

  ```css
  @media (max-width: 767px) {
    .services-grid {
      display: flex;
      overflow-x: auto;
      scroll-snap-type: x mandatory;
      -webkit-overflow-scrolling: touch;
      gap: 16px;
      padding-bottom: 12px;
    }
    .services-grid::-webkit-scrollbar { display: none; }
    .service-card {
      min-width: 82vw;
      flex-shrink: 0;
      scroll-snap-align: start;
    }

    .portfolio-grid {
      display: flex;
      overflow-x: auto;
      scroll-snap-type: x mandatory;
      -webkit-overflow-scrolling: touch;
      gap: 16px;
      padding-bottom: 12px;
    }
    .portfolio-grid::-webkit-scrollbar { display: none; }
    .project-card {
      min-width: 82vw;
      flex-shrink: 0;
      scroll-snap-align: start;
      grid-column: unset;
      grid-template-columns: unset;
    }
  }
  ```

- [ ] **Step 2: Verify on mobile in browser**

  Open Chrome DevTools and set the device to iPhone 14 Pro (390px wide) or use Responsive mode at 375px. Scroll to Services — the three cards should appear as a horizontal swipe row with the second card peeking into view. Swipe (or drag) through them. Repeat for the Work section — all 7 cards should swipe horizontally.

  Also verify desktop is unaffected: switch back to full-width — cards should display as the normal grid.

- [ ] **Step 3: Commit**

  ```bash
  git add index.html
  git commit -m "feat: horizontal swipe carousel for Services and Work cards on mobile"
  ```

---

## Task 8: Update CLAUDE.md with design decisions

**File:** `CLAUDE.md`

Add a new section so future sessions don't re-litigate these decisions.

- [ ] **Step 1: Add animation + scroll + mobile decisions to CLAUDE.md**

  Open `CLAUDE.md`. Find the `## Design decisions made so far` section and add these items to the bullet list:

  ```markdown
  - **Scroll animations:** All sections (except Hero) use `.anim` class for scroll-triggered fade-up. Headings are a single block fade-up. Cards/boxes stagger using `.d1`–`.d4` delay variants. Duration: 750ms. Easing: `cubic-bezier(0.16, 1, 0.3, 1)`. Travel: `44px`. Always include `prefers-reduced-motion` override setting `transition-duration: 0ms`. Hero section is excluded (it's visible on load).
  - **Scroll journey line:** Label accuracy is pin-position driven, not viewport-threshold driven. Section Y positions (`.section-title` element) are measured on load and recalculated on resize using `getBoundingClientRect().top + window.scrollY`, then converted to a journey ratio `(headingY / docHeight) / contactStart`. Labels change when `journeyPct >= s.ratio`.
  - **Mobile card swiping:** Services and Work/Portfolio sections use CSS scroll-snap horizontal scroll at `max-width: 767px`. Pattern: `display: flex; overflow-x: auto; scroll-snap-type: x mandatory;` on the grid container. Each card: `min-width: 82vw; flex-shrink: 0; scroll-snap-align: start`. Scrollbar hidden with `::-webkit-scrollbar { display: none }`. No JS required.
  ```

- [ ] **Step 2: Commit**

  ```bash
  git add CLAUDE.md
  git commit -m "docs: add animation, scroll journey, and mobile swipe decisions to CLAUDE.md"
  ```

---

## Self-Review

**Spec coverage check:**
- ✅ Scroll journey labels fire when pin reaches section heading — Task 1
- ✅ Fade up for headings (single block) — Tasks 3–6
- ✅ Staggered fade up for cards — Tasks 3–6
- ✅ 750ms, `cubic-bezier(0.16, 1, 0.3, 1)`, 44px travel — Task 2
- ✅ `prefers-reduced-motion` — Task 2
- ✅ Hero excluded — not touched in any task
- ✅ Mobile swipe for Services — Task 7
- ✅ Mobile swipe for Work/Portfolio — Task 7
- ✅ Featured card included in swipe — Task 7 (`grid-column: unset` resets it)
- ✅ CLAUDE.md update — Task 8

**No placeholders found.** All code blocks are complete and executable.

**Type consistency:** The class names `.anim`, `.d1`–`.d4`, `.visible` are used consistently across Task 2 (CSS), Task 2 (JS), and Tasks 3–6 (HTML). The JS observer selector `document.querySelectorAll('.anim')` matches the class added in HTML tasks.
