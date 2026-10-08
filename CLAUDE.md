# Pacify — project notes for Claude

Marketing/waitlist site for **Pacify**, a babysitter-matching service in Bangalore. Pure static HTML/CSS/JS — no build step, no `package.json`, no `node_modules`. Deploy by hosting the files as-is (any static host).

## Layout

Everything lives inside the oddly-named folder (keep the name as-is, don't rename it):

```
"Pacify — Babysitter service in Bangalore (1)/"
  index.html     — home page (React-templated, see below)
  blog.html      — plain static HTML page, linked from nav
  sitters.html   — plain static HTML page, NEW, not yet linked from index.html nav (work in progress)
  support.js     — GENERATED runtime, do not hand-edit (see below)
  vendor/        — vendored react.js + react-dom.js (loaded via <script src>, no npm)
  assets/        — images (hashed filenames from the original export + a couple of human-named PNGs added later)
LICENSE          — MIT, copyright ReubenJOSTAR
```

## index.html is special — read before editing

`index.html` is output from an AI site-builder's "dc-runtime" (the comment at the top of `support.js` says *"GENERATED from dc-runtime/src/*.ts — do not edit. Rebuild with `bun build.ts`"*). That source repo isn't present here, so:

- **Never hand-edit `support.js`.** It's the compiled runtime that turns the `<x-dc>...</x-dc>` block into a React app (templating, `{{ expr }}` interpolation, event binding, scroll-reveal animations, etc.).
- The actual page content/markup lives inside `<x-dc> ... </x-dc>` in `index.html`, written as HTML with Handlebars-style `{{ }}` interpolation (e.g. `onClick="{{ setParent }}"`).
- The page's **logic** (state, handlers, EmailJS submit, scroll/reveal wiring) lives in the `<script type="text/x-dc" data-dc-script>` block near the end of `index.html`, as a `class Component extends DCLogic { ... }`.
- Editing plain text/copy/markup inside `<x-dc>` is safe. Adding new interactive behavior means adding methods to that `Component` class and wiring them with `{{ methodName }}`.
- `blog.html` and `sitters.html` are **not** part of this runtime — they're independent static HTML pages with their own inline `<style>`, no React, no `<x-dc>`. Edit them directly like normal HTML.

## Current state / in-progress work

- `sitters.html` and two new assets (`Cozy Arts and Crafts Together.png`, `Joyful Mother-Child Block Building.png`) are **uncommitted**. `sitters.html` is a landing page for recruiting babysitters, but `index.html`'s nav/"Become a Sitter" button still only opens the waitlist modal in sitter mode (`onClick="{{ setSitter }}"`) rather than linking to `sitters.html`. If asked to wire these together, that's the gap to close.
- Only `index.html` links to `blog.html`; nothing currently links to `sitters.html`.

## Business facts (for copy/content changes)

- Pricing: from ₹225/hour/child + a platform fee.
- Contact: phone `+91 88488 73841`, email `query.pacifyco@gmail.com`.
- Waitlist form submits via **EmailJS** (service `service_4ghi7v6`, template `template_hx12jan`, notify email `query.pacifyco@gmail.com`), configured inline in `index.html`'s script block. The EmailJS public key is embedded in the page — this is normal for EmailJS's client-side SDK (it's designed to be public), not a leaked secret.

## Working conventions

- No linter/build/test tooling exists — changes are verified by opening the HTML file directly in a browser.
- Git: single `main` branch, MIT licensed. No `.gitignore` present yet — watch for OS cruft (e.g. `Thumbs.db`) before committing.
