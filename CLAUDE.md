# Markdown Slideshow — Project Rules

A single-page, offline, $0-cost Markdown slideshow tool that runs entirely in the browser.

## Hard Constraints (non-negotiable)

1. **$0 cost forever.** No paid services, paid APIs, paid domains, or paid tooling. Free hosting only (e.g. GitHub Pages or local file).
2. **No backend.** No Node server, no Python server, no serverless functions, no edge workers.
3. **No database.** No SQL, no NoSQL, no remote KV. In-memory state only; optional `localStorage` for user convenience.
4. **No cloud hosting dependencies.** The app must work fully when opened directly from disk (`file://`) with no network.
5. **Single page.** One `index.html` is the entire app. No build step, no bundler, no framework install.
6. **Runs in browser memory.** All parsing, rendering, and state live in the tab. Nothing leaves the user's machine.

## Allowed Tech (and only these)

- **HTML5** — structure.
- **CSS3** — styling (hand-written rules where Tailwind is awkward).
- **Tailwind CSS via CDN** — utility classes. CDN script tag only; no `tailwind.config.js`, no PostCSS, no build.
- **Reveal.js via CDN** — slide engine. Pull `reveal.js`, default theme(s), and any plugins from a CDN (`cdn.jsdelivr.net` or `unpkg`).
- **marked.js via CDN** — Markdown → HTML conversion before handing slides to Reveal.js.
- **Vanilla JS** — glue code. No TypeScript, no React, no Vue, no Svelte, no jQuery.

Anything outside this list requires explicit user approval.

## Required Features

- **Markdown editor pane** — user types or pastes Markdown; slides update live.
- **Slide separator convention** — `---` on its own line splits slides; `--` splits vertical slides (Reveal.js convention).
- **Live preview** — Reveal.js renders the current deck in the same page (split view or toggle).
- **Export to standalone HTML** — one button produces a single `.html` file the user can download. The exported file:
  - Inlines the rendered deck.
  - Pulls Reveal.js + theme CSS from CDN (so the export stays small) **or** inlines them (user-selectable; default = CDN for size, with a "fully offline export" toggle that inlines everything).
  - Opens and presents correctly by double-clicking the file — no server needed.
  - Contains no references to the editor UI, no Tailwind, no marked.js.

## Nice-to-haves (only if asked)

- Theme picker (Reveal.js built-in themes).
- Speaker notes via `Note:` blocks.
- Drag-and-drop `.md` file to load.
- `localStorage` autosave of the current draft.

## Coding Rules

- One file: `index.html`. Inline `<style>` and `<script>` are fine; split into `app.js` / `styles.css` only if the file gets unwieldy (>~800 lines).
- No comments explaining what code does — only why, when non-obvious.
- No frameworks, no transpilers, no package managers. If `npm` or `package.json` shows up, something has gone wrong.
- All third-party scripts must be loaded from a public CDN with `integrity` + `crossorigin` attributes when the CDN provides SRI hashes.
- Sanitize Markdown rendering (marked.js has built-in escaping; don't disable it).

## Out of Scope

- User accounts, login, sync.
- Collaboration / real-time editing.
- Analytics, telemetry, tracking.
- Image upload to a server (users can paste image URLs or data URIs).
- Any feature that requires a backend to function.

## Definition of Done

A change is done when:
1. `index.html` opens by double-click and works with no network (CDN scripts cached or replaced with offline fallbacks during testing).
2. Editing Markdown updates the preview without errors in the console.
3. Export produces a `.html` file that, when opened standalone, presents the deck correctly.
4. No new dependencies have crept in beyond the Allowed Tech list.
