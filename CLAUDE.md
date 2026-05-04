# Rezonate Website — Claude Instructions

Read `README.md` first for full project context before doing anything.

---

## Workflow for every change request

When the user asks to make a change to the website, follow this process:

### Step 1 — Ask clarifying questions before touching any code

Ask 2–4 targeted questions to make the best design decision. Focus on:
- **Copy/content** — exact wording, or should you suggest something?
- **Visual style** — tone, size, colour, layout preference
- **Placement** — which section, above or below what
- **Scope** — is this a small tweak or a bigger redesign of a section?

Do not ask about things already decided in the README. Keep questions short and specific. Wait for the user to answer before writing any code.

### Step 2 — Make changes directly to `index.html`

Use the `Edit` tool to make targeted changes directly to `/Users/zainraza/Desktop/rezonate-website/index.html`. Do not output the full file. Do not ask the user to copy/paste or upload anything.

After editing, open the file in the browser so the user can preview the changes:

```
open /Users/zainraza/Desktop/rezonate-website/index.html
```

If the change involves scroll behaviour or animations (e.g. the journey line), use a local server instead to avoid browser `file://` quirks:

```
cd /Users/zainraza/Desktop/rezonate-website && python3 -m http.server 8080
```

Then tell the user to visit `http://localhost:8080` in their browser. After they confirm it looks right, stop the server.

Briefly describe what changed and ask if anything needs adjusting.

---

## Key rules

- Always work from the actual `index.html` file on disk — read it before editing
- Only change what the user asked for — don't refactor or clean up surrounding code
- Match the existing brand: navy `#2A4DA8`, red `#D63028`, cream `#F7F6F2`, Epilogue headings, DM Sans body
- Section labels are always red — no per-section colour overrides
- The user does not write code — Claude writes everything
- Keep the file named exactly `index.html`
- After changes are saved, commit via terminal (`git add index.html && git commit -m "..."`) then the user clicks **Sync** in VS Code to push — GitHub Pages deploys automatically

---

## Design decisions made so far

- **Scroll animations:** All sections (except Hero) use `.anim` class for scroll-triggered fade-up. Headings are a single block fade-up. Cards/boxes stagger using `.d1`–`.d4` delay variants (0.10s / 0.22s / 0.34s / 0.48s). Duration: 750ms. Easing: `cubic-bezier(0.16, 1, 0.3, 1)`. Travel: `44px`. Always include `prefers-reduced-motion` override setting `transition-duration: 0ms` and `transition-delay: 0ms`. Hero section excluded (visible on load). Observer: single `IntersectionObserver` (`animObserver`) at threshold 0.1.
- **Scroll journey line:** Label accuracy is pin-position driven, not viewport-threshold driven. Each section's `.section-title` Y position is measured on load and recalculated on resize using `getBoundingClientRect().top + window.scrollY`, converted to `s.ratio = (headingY / docHeight) / contactStart`. Labels change in `updateJourney()` when `journeyPct >= s.ratio`. Guard `if (!contactStart) return` prevents division-by-zero edge case.
- **Mobile card swiping:** Services (`.services-grid`) and Work/Portfolio (`.portfolio-grid`) sections use CSS scroll-snap horizontal scroll at `max-width: 767px`. Grid pattern: `display: flex; overflow-x: auto; scroll-snap-type: x mandatory; overscroll-behavior-x: contain; scrollbar-width: none; gap: 12px; padding-right: 24px`. Each card: `width: calc(100% - 22px); min-width: 0; flex-shrink: 0; scroll-snap-align: start`. Use `width` (not `min-width`) and `100%` relative to the container — do NOT use `vw` units, they are unreliable on iOS Safari. Scrollbar hidden with `::-webkit-scrollbar { display: none }`. `.project-card` gets `grid-column: unset; grid-template-columns: unset` to neutralise featured card grid properties in flex context. **Dot indicators:** `.swipe-dots` div added after each grid (ids `servicesDots`, `portfolioDots`). Dots built and updated via JS (`initSwipeCarousels()`). Active dot: navy `#2A4DA8`, inactive: `#d0d0d0`. **Nudge animation:** On first viewport entry (IntersectionObserver at threshold 0.6), grid scrolls to 44px then smoothly back to 0 after 380ms — signals to user that content is swipeable.
