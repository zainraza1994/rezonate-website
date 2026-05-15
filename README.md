# Rezonate Website — Project Context

Use this file to get up to speed before making changes to the Rezonate website. Read everything before touching any code.

---

## Who I am

**Name:** Zain Raza  
**Company:** Rezonate Ltd  
**Role:** Founder & Lead Service Designer — contracts outside IR35  
**Location:** United Kingdom  
**Email:** zainrazacoop@gmail.com 
**LinkedIn:** https://www.linkedin.com/in/zain-raza-25296778/

---

## What Rezonate does

At Rezonate, we help organisations design and deliver impactful services that put innovation and people at the heart. Our aim is to build services that fulfil the needs of their users and deliver long lasting value for our clients. We build deep relationships with clients, working collaboratively from discovery to delivery.

Led by Zain Raza — an experienced designer with deep expertise across research, UX, service design, and strategic thinking across public and private sectors.

**Services offered:**
- Strategic Design
- Service Design
- End-to-End Design & Delivery

**Clients / experience includes:**
Kainos, Investigo Government Solutions, Department for Work and Pensions (DWP), Lloyds Banking Group, Co-op Bank, NHS, Department for International Trade, Hippo Digital, EY Seren

---

## Brand identity

- **Primary colour:** Navy — `#2A4DA8`
- **Navy dark (hover):** `#1A3580`
- **Accent/red:** `#D63028`
- **Background:** Cream — `#F7F6F2`
- **Dark (text/sections):** `#0C0C0C`
- **Mid grey:** `#555563`
- **Border:** `#E0DFD9`
- **Light blue (tags/icons):** `#EEF2FB`
- **Heading font:** Epilogue (Google Fonts) — weights 400, 600, 700, 800, 900
- **Body font:** DM Sans (Google Fonts) — weights 300, 400, 500, 600
- **Tone:** Polished, bold, professional but with personality. Designed to look like a designer made it.
- **Logo:** Text-based wordmark in the nav — `Re` + red `z` + `onate` using `.logo-text` CSS class. Epilogue 800, navy colour, red accent on the z. No image file used in nav.

---

## Website purpose

The site serves two purposes:
1. Company showcase — what Rezonate does, services, approach, sector experience
2. Portfolio / CV — case studies Zain can send to potential clients as a link

---

## Tech stack & hosting

| Thing | Detail |
|---|---|
| Built with | Single HTML file (`index.html`) — HTML, CSS, vanilla JavaScript |
| Code editor | VS Code (user is new to coding — Claude writes all code) |
| Version control | GitHub — repo: `rezonate-website` (user: `zainraza1994`) |
| Hosting | GitHub Pages — free, served directly from the `main` branch |
| Deployment | Push to `main` → GitHub Pages auto-deploys in ~1–3 minutes |
| Custom domain | `rezonatedesign.co.uk` — registered on Cloudflare Registrar (~£5.50/yr, no renewal hikes) |
| Live URL | `https://rezonatedesign.co.uk` |
| DNS | Cloudflare (DNS-only mode — grey cloud, not proxied — so GitHub Pages handles HTTPS) |
| CNAME file | `/CNAME` in repo root contains `rezonatedesign.co.uk` — do not delete |

**Update workflow:**
1. User tells Claude what to change
2. Claude edits `index.html` directly using the Edit tool
3. Claude commits via terminal (`git add index.html && git commit -m "..."`)
4. User clicks **Sync** in VS Code to push to GitHub
5. GitHub Pages auto-deploys — live in ~1–3 minutes
6. Hard refresh in browser (`Cmd+Shift+R`) if the old version shows

**Critical:** The file must always be named exactly `index.html` and must live in the root of the `main` branch — GitHub Pages won't find it otherwise.

**Important for Claude:** Always read the current `index.html` from disk at the start of a session before making any changes — work from the file on disk, not from memory.

---

## Site structure (sections in order)

1. **Nav** — Fixed. Text logo (top left), links: About / Services / Work / Get in touch (pill CTA). Transparent until scroll, then white with border.
2. **Hero** — Full viewport height. Left-aligned. Label, H1, subtitle, two CTA buttons.
3. **About** — Dark (`#0C0C0C`) background. Brand mission, 4 design pillars.
4. **Services** — Cream background. 3 service cards (01, 02, 03).
5. **Portfolio / Work** — Cream background. 6 project cards in a horizontal-scroll rail (desktop + mobile). Prev/next controls + counter on desktop; dot indicators on mobile. Each card has a thematic inline SVG animation.
6. **How we work / Approach** — **Black (`#000`)** background. V3 pinned scrollytelling — `450vh` tall section, sticky inner stage, 4 crossfading chapters (Discover / Define / Develop / Deliver) with morphing background shapes and progress segments.
7. **Trusted by** — Cream background. Client logo strip (4 clients).
8. **Contact** — Dark background. Contact details + enquiry form.
9. **Footer** — Dark background. Copyright + text logo.

---

## Scroll journey feature

A decorative wavy dotted line runs down the left edge of the page (desktop only):

- **Hidden on mobile** (`display: none` below 900px)
- Invisible until user scrolls ~60px down
- Starts from the hero H1 heading position
- A red location pin (matching the logo) travels down the wavy path as the user scrolls
- The trail behind the pin is a blue dotted line that fills in as you scroll
- A small section label fades in next to the pin (About / Services / Work / Approach / Experience)
- The whole feature fades out before the Contact section
- Built using an SVG path with `stroke-dashoffset` animation, driven by scroll position

---

## Section label styling

All section labels (the small uppercase red labels like "About Rezonate", "How we work") use the global `.section-label` class — they are **all red** (`var(--red)`) with a red line before them. There are no per-section colour overrides for labels except on the contact section (which is already red). Do not add section-specific label colour overrides.

---

## Services — current content & tags

### 01 — Strategic Design
Setting the direction. Strategic-level work to define where the service needs to go.  
**Tags:** Vision setting · Service strategy · Design leadership

### 02 — Service Design
Designing the experience. Discovery to detailed design — user journeys, pain points, solutions.  
**Tags:** Journey mapping · Prototyping · Blueprinting · Systems thinking

### 03 — End-to-End Design & Delivery
From concept to live product. Leading multidisciplinary teams through full build, testing and launch.  
**Tags:** UX Research · Agile delivery · UX Design · Roadmapping

---

## Portfolio projects — current state on site

All 6 cards are in a horizontal-scroll rail. No featured card. Each has a `.project-diagram` SVG animation.

### 1. Universal Credit Fraud & Error
**Client:** Department for Work and Pensions | **Sector:** Public Sector  
**Tags:** Design Leadership · Service Design · E2E Design · AI Integration  
**Stats:** £500m+ saved · 5,000+ agents using the service  
**Diagram:** AI scan beam sweeping a grid of claim records; anomaly flagged with red pulse

### 2. Automated Vehicles Act Data Service
**Client:** Kainos | **Sector:** Public Sector  
**Tags:** Service Design · GDS Assessment · AI Tools  
**Diagram:** Car on curved road with sensor pings; data uplink dots to DATA SVC hub

### 3. DSIT — Matrix Programme
**Client:** Investigo Government Solutions | **Sector:** HR/Finance  
**Tags:** Design Leadership · Transformation · Op Model  
**Diagram:** 3×4 dept/function matrix; cells flash navy/red in sequence

### 4. Bus Open Data Service (BODS)
**Client:** Kainos | **Sector:** Public Sector  
**Tags:** Service Design · Service Mapping  
**Diagram:** Bus moving along route with named stops; animated dashes + live ETA panel

### 5. Kickstart Scheme — Youth Employment
**Client:** Department for Work and Pensions | **Sector:** Public Sector  
**Tags:** Service Design · E2E Design/Delivery  
**Stats:** ↓ 94% processing time · ↑ Vacancies  
**Diagram:** BEFORE/AFTER bars (90 min vs 5 min) with animated stopwatches + ↓ 94% stat

### 6. Re-imagine Branch Services
**Client:** Lloyds Bank | **Sector:** Financial Services  
**Tags:** User Testing · Service Design · Transformation  
**Stats:** ↑ Digital awareness · ↓ Branch footfall  
**Diagram:** Dual sparklines — branch (red, declining) and digital (navy, rising) crossing

---

## Trusted by — current state

The old "Sectors" section has been replaced with a "Trusted by" client logo strip. It is a 4-column grid with a bordered card style. Section label is "Experience", heading is "Trusted by".

| Client | Status |
|---|---|
| Department for Work and Pensions | ✅ Embedded as base64 JPEG (dark-background tile — `client-logo--dark` class with black background) |
| Kainos | ⚠️ Hotlinked from freelogovectors.net — **needs replacing with a properly embedded file** |
| Lloyds Banking Group | 🔲 Still text wordmark — needs a real logo file |
| Investigo Government Solutions | 🔲 Still text wordmark — needs a real logo file |

**To embed a new logo:** User uploads the file → Claude converts to base64 → embeds in the `client-logo` div as `<img>`. If the logo has a dark/black background (like DWP), add the `client-logo--dark` class to the container div, which applies a `#0c0c0c` background.

---

## How we work / Approach section — current state (V3)

- **Background:** Black (`#000`)
- **Layout:** Pinned scrollytelling — section is `450vh` tall. Inner `.approach-stage` is `position: sticky; top: 0; height: 100vh`.
- **Meta bar:** "How we work" label + chapter counter (`01 · Chapter 1 of 4`) + 4 red progress segments
- **Chapters:** 4 crossfading `<article class="approach-chapter">` elements — 01 Discover / 02 Define / 03 Develop / 04 Deliver. Active chapter fades in; inactive is `opacity: 0; transform: translateY(28px)`.
- **Background shapes:** Abstract SVG swaps per chapter — radar rings (Discover), nested rectangles (Define), overlapping circles (Develop), triangle (Deliver).
- **Pills:** Row of 4 shortcut buttons inside each chapter for jumping to a specific chapter.
- **Mobile (≤ 900px):** Pinning disabled. Section collapses to `height: auto`. Chapters stack vertically as cards with a top border. Pills and scroll hint hidden.

---

## Design decisions made so far

- **Font:** Epilogue (headings) + DM Sans (body)
- **Logo:** Text-based `.logo-text` wordmark — `Re` + red `z` + `onate`
- **Hero:** Single column, left-aligned, no stat tiles
- **Client ticker bar:** Removed
- **Nav links:** Lowercase, red underline on hover, pill-shaped CTA
- **Journey line:** Subtle, wavy, desktop-only scroll indicator
- **Hero heading:** "Design services" / "that resonate." — split across two lines
- **Section labels:** All red (`var(--red)`) universally — no per-section overrides
- **Approach background:** Changed from navy to black
- **Sectors section:** Replaced entirely with "Trusted by" client logo strip

---

## Things still to do / not yet decided

- [x] **Selected Work section** — redesigned as a horizontal-scroll rail with thematic SVG animations. 6 cards, desktop controls, mobile dot indicators.
- [ ] **Trusted by logos** — embed proper files for Kainos (replace hotlink), Lloyds Banking Group, and Investigo Government Solutions
- [ ] **Contact form** — replace with a real working form (Netlify Forms or Formspree)
- [ ] **Real contact details** — add real email address and LinkedIn URL (currently placeholders)
- [ ] **Hero** — decide what goes where the stat tiles used to be
- [x] **Custom domain** — `rezonatedesign.co.uk` live. Registered on Cloudflare. DNS A records + CNAME set. CNAME file in repo root. HTTPS enforced via GitHub Pages.
- [ ] **Mobile navigation** — hamburger menu (nav links currently hidden on mobile)

---

## How to continue

1. Read this whole file first
2. Read the current `index.html` from disk at the start of each session — never work from memory
3. Write all code — the user is new to coding and does not write any code themselves
4. Claude edits `index.html` directly using the Edit tool, commits via terminal, user clicks Sync in VS Code
5. Ask design questions before building when a decision needs to be made
6. When embedding logos: convert uploaded image files to base64 using Python, embed as `data:image/...;base64,...` in the HTML so there are no external dependencies

---

*Last updated: May 2026*
# rezonate-website
Rezonate Ltd Website
