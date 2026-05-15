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
- After changes are saved, commit via terminal (`git add index.html && git commit -m "..."`) then the user clicks **Sync** in VS Code to push — GitHub Pages deploys automatically in ~1–3 minutes
- Live site is at `https://rezonatedesign.co.uk` — registered on Cloudflare, DNS-only mode (grey cloud). Do not change DNS proxy settings.
- A `CNAME` file in the repo root contains `rezonatedesign.co.uk` — never delete or overwrite it

---

## Design decisions made so far

- **Scroll animations:** All sections (except Hero) use `.anim` class for scroll-triggered fade-up. Headings are a single block fade-up. Cards/boxes stagger using `.d1`–`.d4` delay variants (0.10s / 0.22s / 0.34s / 0.48s). Duration: 750ms. Easing: `cubic-bezier(0.16, 1, 0.3, 1)`. Travel: `44px`. Always include `prefers-reduced-motion` override setting `transition-duration: 0ms` and `transition-delay: 0ms`. Hero section excluded (visible on load). Observer: single `IntersectionObserver` (`animObserver`) at threshold 0.1.
- **Scroll journey line:** Label accuracy is pin-position driven, not viewport-threshold driven. Each section's `.section-title` Y position is measured on load and recalculated on resize using `getBoundingClientRect().top + window.scrollY`, converted to `s.ratio = (headingY / docHeight) / contactStart`. Labels change in `updateJourney()` when `journeyPct >= s.ratio`. Guard `if (!contactStart) return` prevents division-by-zero edge case.
- **Services mobile carousel:** Services (`.services-grid`) uses CSS scroll-snap horizontal scroll at `max-width: 767px` only. Dot indicator (`servicesDots`) built by `initSwipeCarousels()`. Nudge animation on first viewport entry (scrolls 44px then back). Active dot: navy `#2A4DA8`, inactive: `#d0d0d0`.
- **Portfolio horizontal rail (desktop + mobile):** The `.portfolio-grid` is a horizontal flex scroll on ALL viewports (not just mobile). Cards are `flex: 0 0 520px; scroll-snap-align: start; gap: 16px`. The rail breaks out to viewport edges via `.portfolio-rail { margin: 0 calc(50% - 50vw - 8px); padding: 4px 0 4px max(48px, calc(50vw - 600px)); }`. Desktop controls: `.portfolio-controls` with `#pcPrev` / `#pcNext` buttons, `#pcCurrent` / `#pcTotal` counter, `.pc-progress` bar driven by CSS custom properties `--pc-w` and `--pc-x`. On mobile (≤ 767px): controls hidden, rail margin collapses to `-24px`, cards widen to `calc(100vw - 46px)`, dot indicator shown. `initSwipeCarousels()` drives the rail on all viewports; keyboard arrow nav supported on desktop. Each card has a `<div class="project-diagram">` with an inline SVG animation thematic to the project.

---

## Selected Work section — current state (as of May 2026)

The `.portfolio` section (`id="work"`) is a **horizontal-scroll rail** on all viewports. Each card is `flex: 0 0 520px`. Desktop shows prev/next round buttons + `01 / 06` counter + progress bar. Mobile shows dot indicators only.

Each card has:
- `.project-card-top` — project number (e.g. `01`) + `+` expand glyph
- `.project-diagram` — `192px` tall panel with a thematic inline SVG animation
- sector label, client, title, description, skill tags, optional `.project-meta` stats

### Current cards (in order)

1. **Universal Credit Fraud & Error** — Client: DWP. Tags: Design Leadership, Service Design, E2E Design, AI Integration. Stats: £500m+ saved, 5,000+ agents. Diagram: AI scan beam sweeping a 5×4 grid of claim records, flagging an anomaly with a red pulse.
2. **Automated Vehicles Act Data Service** — Client: Kainos. Tags: Service Design, GDS Assessment, AI Tools. Diagram: Car moving along a curved road with sensor pings; data uplink dots flowing to a DATA SVC hub.
3. **DSIT — Matrix Programme** — Client: Investigo Government Solutions. Sector: HR/Finance. Tags: Design Leadership, Transformation, Op Model. Diagram: 3-row × 4-col dept/function matrix; cells flash navy/red in sequence.
4. **Bus Open Data Service (BODS)** — Client: Kainos. Tags: Service Design, Service Mapping. Diagram: Bus moving along a route with named stops; animated dashes + live ETA panel.
5. **Kickstart Scheme — Youth Employment** — Client: DWP. Tags: Service Design, E2E Design/Delivery. Stats: ↓ 94% processing time, ↑ Vacancies. Diagram: BEFORE/AFTER bar comparison (90 min vs 5 min) with animated stopwatches + ↓ 94% stat.
6. **Re-imagine Branch Services** — Client: Lloyds Bank. Sector: Financial Services. Tags: User Testing, Service Design, Transformation. Stats: ↑ Digital, ↓ Footfall. Diagram: Dual sparklines — branch (red, declining) and digital (navy, rising) — drawing in and crossing.

---

## Approach section — current state (as of May 2026)

The `.approach` section is a **V3 pinned scrollytelling** experience (`height: 220vh`). The inner `.approach-stage` is `position: sticky; top: 0; height: 100vh` and pins while the user scrolls through 4 steps.

- **Meta bar:** Section label ("How we work") + step counter (`01 · Step 1 of 4`) + 4 red progress segments. The label says "Step", not "Chapter".
- **Intro paragraph:** A persistent `.approach-intro` paragraph sits between the meta bar and the body at all times — it does not change as steps scroll. Text: "We use a trusted design methodology throughout every engagement — ensuring we design the right thing, then design it right. Collaborative, evidence-based, and always people-centred." Styled: DM Sans, 19px, `rgba(247,246,242,0.68)`, `max-width: 500px`, `margin-bottom: 28px`. Visible on both desktop and mobile.
- **Body:** Two-column grid. Left: giant numeral (01–04). Right: title, description, pill shortcuts for jumping
- **Background shape:** Abstract SVG (`data-shape="0"–"3"`) swaps per step — radar / rectangles / overlapping circles / triangle
- **JS:** `initApproachScrollytelling()` drives step swaps on scroll. Mobile (≤ 900px): section collapses to `height: auto`, steps stack vertically as cards, pinning disabled. Pills hidden on mobile.
- **Step copy:** 01 Discover / 02 Define / 03 Develop / 04 Deliver
