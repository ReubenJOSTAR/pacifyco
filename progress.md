# Progress Log

## 2026-10-08

### Git / GitHub
- Confirmed `origin` remote already pointed at `https://github.com/ReubenJOSTAR/pacifyco.git`.
- Pushed the pending `bd665e7` commit (sitters page + nav/footer links) to `origin/main`.
- No credential helper was configured, which blocked the first push attempt. Fixed by setting
  `git config --global credential.helper manager` (Git Credential Manager, bundled with Git for
  Windows) and re-running the push with `GIT_TERMINAL_PROMPT`/`GCM_INTERACTIVE` re-enabled so GCM
  could do its browser-based login. Credentials are now cached — future pushes shouldn't need this.

### SEO meta tags
- Added `canonical`, Open Graph, and Twitter Card tags to all three pages:
  - `index.html` — `og:image`/`twitter:image` use `assets/hero-2.jpg` (homepage hero photo).
  - `blog.html` — same `hero-2.jpg` as a brand fallback, since the `POSTS` array is currently
    empty (no real posts published yet, just a commented-out example template).
  - `sitters.html` — uses `assets/svc-out-2.jpg` (sitter blowing bubbles with a toddler) as a more
    relevant image for the sitter-recruitment page.
- All canonical/`og:url`/image URLs point at the confirmed production domain
  `https://www.pacifyco.in/`.
- Committed as `f38e0f9` ("Add SEO meta tags and update pricing to Rs199/hour") and pushed to
  `origin/main`.

### Pricing copy update
- Changed pricing from ₹225/hour/child to ₹199/hour/child in two places on `index.html`:
  the meta description, and the FAQ answer ("How much does a babysitter in Bangalore cost?").
  No other ₹225 references existed anywhere else in the site.
- Note: `CLAUDE.md`'s "Business facts" section still says ₹225/hour/child — that file should be
  updated to ₹199 to stay accurate, but wasn't touched this session (not explicitly requested).

### Deployment / hosting
- Confirmed the publish directory for whatever static host is used is
  `Pacify — Babysitter service in Bangalore (1)/` itself (not the repo root), so relative asset
  paths like `assets/hero-2.jpg` correctly resolve at `https://www.pacifyco.in/assets/hero-2.jpg`
  — matching what's hardcoded in the new meta tags. No path mismatch.

### Open issue — WhatsApp link preview not showing a card
- User reported sharing `https://www.pacifyco.in/` on WhatsApp doesn't produce a preview card.
- Image file sizes ruled out as the cause (`hero-2.jpg` 208KB, `svc-out-2.jpg` 353KB — both well
  within normal limits).
- Most likely causes, not yet confirmed:
  1. WhatsApp cached a stale "no preview" result from before the OG tags existed (very common —
     WhatsApp has no manual re-scrape button; workaround is testing with a cache-busting query
     string, e.g. `?v=1`).
  2. The live site may not actually have the latest deploy yet.
  3. Something host-side (SSL, robots.txt, etc.) blocking the crawler — undiagnosed.
- Next step handed to user: check the live page source for the `og:` tags, confirm the image URL
  loads directly in a browser, and run the URL through Facebook's Sharing Debugger
  (https://developers.facebook.com/tools/debug/, "Scrape Again") since it gives actual error
  output, unlike WhatsApp. Awaiting results before diagnosing further.
