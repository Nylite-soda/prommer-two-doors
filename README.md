# prommer.net: "two doors"

A single static page (`index.html`), with no build step. GSAP 3.12.5 loads from cdnjs.

- `/` is the landing page ("What brings you here?")
- `/#build` is Door 1, "See what Thomas builds" (founders/operators)
- `/#book` is Door 2, "Book Thomas" (press/podcasts)

## Before you deploy: set the contact URL

Open `index.html` and edit the first line of the main `<script>`:

```js
const CONTACT_URL = 'https://www.linkedin.com/in/thomasprommer'; // placeholder
```

Every "Talk to Thomas" and "Send a speaking enquiry" button reads from this constant.
(The HTML `href`s hold the same placeholder as a no-JS fallback. Update them too if you
care about visitors without JavaScript.)

## Editing content

- **Press topics:** the `PRESS_TOPICS` array, right under `CONTACT_URL`. Each item has
  `outlet`, `type`, `date` (ISO), `title` and `url`. Items are sorted newest first within each topic.
- **Bios:** the three `<div class="bio-text">` blocks in Door 2. The Copy buttons and word counts
  read the text from these blocks, so editing the HTML is enough.
- **Cards, recognition chips, links:** plain HTML in the `#build` section.

## Run locally

```bash
python -m http.server 5173
# then open http://localhost:5173/
```

Any static server works (`npx serve .`). Use `localhost` rather than `file://`: the Clipboard API
needs a secure context. The page falls back to a legacy copy method anyway.

## Deploy

**Netlify**
- Drag and drop: app.netlify.com → *Add new site* → *Deploy manually* → drop this folder.
- CLI: `npx netlify-cli deploy --dir . --prod`
- Git: connect the repo, leave the build command empty, and set the publish directory to `.`

**Vercel**
- CLI: `npx vercel --prod` (choose "Other" as the framework, with no build command and output directory `.`)
- Git: import the repo, set Framework Preset to *Other*, and leave Build and Output empty.

Hash routing needs no rewrite rules.

## Behaviour notes

- Browser back/forward and the in-page "← Back" button both work. If a visitor deep-links
  straight to `#build` or `#book`, "Back" takes them to the landing page instead of off the site.
- `prefers-reduced-motion: reduce` switches off the intro stagger, the view transitions and
  the flywheel rotation. Views then switch instantly.
- If the GSAP CDN fails, the page still works, just without animation.
- Keyboard: every door, button and link can be focused, with a visible orange focus ring.
  On each view change, focus moves to the new view's heading, and a live region
  announces copy results.
