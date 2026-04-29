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
| Version control | GitHub — repo: `rezonate-website` |
| Hosting | Netlify — live at `rezonate.netlify.app` |
| Deployment | GitHub linked to Netlify. Push to GitHub → Netlify auto-deploys in ~30 seconds |

**Update workflow:**
1. User tells Claude what to change
2. Claude produces updated `index.html`
3. User goes to GitHub repo → Add file → Upload files → replaces `index.html`
4. Netlify auto-deploys

**Critical:** The file must always be named exactly `index.html` — Netlify won't find it otherwise.

**Important for Claude:** Always ask the user to upload the current `index.html` at the start of a new session before making any changes — the file in memory here may be behind the live version.

---

## Site structure (sections in order)

1. **Nav** — Fixed. Text logo (top left), links: About / Services / Work / Get in touch (pill CTA). Transparent until scroll, then white with border.
2. **Hero** — Full viewport height. Left-aligned. Label, H1, subtitle, two CTA buttons.
3. **About** — Dark (`#0C0C0C`) background. Brand mission, 4 design pillars.
4. **Services** — Cream background. 3 service cards (01, 02, 03).
5. **Portfolio / Work** — Cream background. 7 project cards including 1 featured full-width card.
6. **How we work / Approach** — **Black (`#000`)** background. 4-step process.
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

### 1. Universal Credit — Fraud & Error ⭐ FEATURED
**Client:** Department for Work and Pensions  
**Sector:** Public Sector  
**Description:** Provided service design leadership for fraud and error in Universal Credit, including leading a multidisciplinary team to design a scalable, efficient fraud-prevention service. The work leveraged AI and innovation to proactively target risk, all grounded in user-centred thinking across a complex, high-stakes environment.  
**Stats:** £500m+ saved · £1.4bn target this year · 5,000+ agents · Multi-dept aligned  
**Tags:** Design Leadership · Service Design · UX Research · UX Design · AI Integration

### 2. Bus Open Data Service (BODS)
**Client:** Kainos  
**Sector:** Public Sector / Transport  
**Description:** Commissioned to reimagine BODS — transforming it from a purely data-focused platform into a user-centred experience. Facilitated collaboration across government, data and technical teams.

### 3. Kickstart Scheme — Youth Employment
**Client:** DWP / Hippo Digital  
**Sector:** Public Sector  
**Stats:** 90 → 5 min processing time · 50 employer & agent interviews

### 4. An 'Investor First' Experience
**Client:** Department for International Trade  
**Sector:** Government / Trade  
**Stats:** 50 investor interviews · 8 investor segments defined

### 5. Redesigning Business Banking
**Client:** Major UK High Street Bank (anonymised)  
**Sector:** Financial Services  
**Stats:** 15 customer interviews · increased website traffic

### 6. Re-imagining Insurance Claims
**Client:** Top UK Insurer (anonymised)  
**Sector:** Insurance  
**Stats:** 60 user interviews · 1,000 survey participants

### 7. A Leading Mobile Savings Proposition
**Client:** Building Society / EY Seren  
**Sector:** Financial Services  
**Stats:** 14 customer interviews · 6 personas developed

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

## How we work / Approach section — current state

- **Background:** Black (`#000`)
- **Intro text:** "We use a trusted design methodology throughout every engagement — ensuring we design the right thing, then design it right. Collaborative, evidence-based, and always people-centred."
- **Step cards:** 4 steps — Discover, Define, Develop, Deliver. Boxes at `rgba(255,255,255,0.08)` opacity with `rgba(255,255,255,0.18)` border. Numbers at `rgba(255,255,255,0.18)`.
- **Layout:** Header uses `align-items: flex-end` so intro text aligns to bottom of heading (consistent with other sections).

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

- [ ] **Selected Work section** — review and update content/copy on the remaining 6 project cards (cards 2–7). May need to revisit descriptions, stats and tags.
- [ ] **Trusted by logos** — embed proper files for Kainos (replace hotlink), Lloyds Banking Group, and Investigo Government Solutions
- [ ] **Contact form** — replace with a real working form (Netlify Forms or Formspree)
- [ ] **Real contact details** — add real email address and LinkedIn URL (currently placeholders)
- [ ] **Hero** — decide what goes where the stat tiles used to be
- [ ] **Custom domain** — currently `rezonate.netlify.app`
- [ ] **Mobile navigation** — hamburger menu (nav links currently hidden on mobile)

---

## How to continue

1. Read this whole file first
2. Ask the user to upload the latest `index.html` — always work from the file they provide, not from memory
3. Write all code — the user is new to coding and does not write any code themselves
4. Always produce a complete updated `index.html` for the user to download and upload to GitHub
5. Ask design questions before building when a decision needs to be made
6. When embedding logos: convert uploaded image files to base64 using Python, embed as `data:image/...;base64,...` in the HTML so there are no external dependencies

---

*Last updated: end of session 2*
# rezonate-website
Rezonate Ltd Website
