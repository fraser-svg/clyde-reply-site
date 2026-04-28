# Clyde Reply — site

Production landing page. Single static HTML, self-hosted Geist Variable font.

## Files

  index.html               — the page (one file, no build step)
  fonts/Geist-Variable.woff2 — self-hosted variable font (68KB)
  DESIGN.md                — design system spec (read before editing)
  README.md                — this file

## Local preview

  python3 -m http.server 8000
  open http://localhost:8000

## Deploy

Cloudflare Pages, drag-and-drop the directory or `wrangler pages deploy .`

## Editing copy

Read these in order:

  1. /Users/foxy/jordan-platten-study/lp/clydereply-home-v2-uk.md (the spec)
  2. ./DESIGN.md (the design system)
  3. /Users/foxy/.claude/plugins/soup/references/voice-rules.md (the voice)

Don't edit copy without re-running the Dual Readership Path audit (read just the headlines top-to-bottom; they must tell one coherent story).

## Outstanding tasks (from DEPLOY hand-off)

  - [ ] Email forwarding for hello@clydereply.co.uk → fraserklw@gmail.com (5-min ImprovMX signup)
  - [ ] Calendly URL for #book secondary CTA (currently anchors to final-CTA section)
  - [ ] Replace placeholder favicon with a real SVG mark
  - [ ] Hero image (when client 1 closes — for now no image is better than a stock one)
  - [ ] Companies House registration → swap "registration pending" → real number
  - [ ] ICO registration → same
