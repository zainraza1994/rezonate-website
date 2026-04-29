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
- Keep the file named exactly `index.html` (Netlify requirement)
- After changes are saved, the user pushes to GitHub and Netlify auto-deploys in ~30 seconds
