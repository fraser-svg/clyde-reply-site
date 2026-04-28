# Design System for Clyde Reply

> Single-page marketing site. Audience: independent UK KBB showroom owners (avatar = Iain, see /customer/avatar.md). Aesthetic stance: editorial-minimal, operator-led. Anti-template, anti-slop (per impeccable). The page is the showroom — quiet, considered, no flash.

---

## 1. Visual Theme & Atmosphere

Editorial-modern. Warm-paper neutrals tinted toward Clyde-water blue. Single sans typeface, generous line-height, one column wide enough to read like a letter. Type IS the design — imagery is inline punctuation, not décor.

The page should read like a confident handwritten note from a Lenzie operator to a Cheshire showroom owner. Not "premium". Not "world-class". Quiet, useful, clear. If it could plausibly have been printed on heavy paper and posted, we're in the right neighbourhood.

No: gradients, drop shadows on cards, glow effects, glassmorphism, neumorphism, large hero photos that span the viewport, parallax, scroll-jacking, stock images of laptops, "AI" iconography, geometric blob shapes, purple anything.

Yes: white space as a design element, OKLCH neutrals tinted toward brand hue (no pure #000 / #fff), one accent colour used sparingly, type-driven hierarchy via colour and weight (not size jumps), inline imagery at body-text scale, semantic HTML.

---

## 2. Color Palette & Roles

OKLCH only. All neutrals tinted toward the accent hue (240° — Clyde-water blue) at low chroma so the whites feel paper-warm and the darks feel ink, not pixel.

| Token        | OKLCH                  | Hex fallback | Role                                              |
|--------------|------------------------|--------------|---------------------------------------------------|
| `--bg`       | oklch(99% 0.005 240)   | `#fdfdff`    | Page background. Warm paper, NOT pure white.      |
| `--surface`  | oklch(97% 0.008 240)   | `#f6f7fa`    | Inline tiles, FAQ cards, subtle separators.       |
| `--fg`       | oklch(22% 0.015 240)   | `#23262d`    | Primary text. Ink, NOT pure black.                |
| `--fg-quiet` | oklch(56% 0.012 240)   | `#7d808a`    | Secondary text, footer, ghosted lead-in lines.    |
| `--fg-faint` | oklch(78% 0.008 240)   | `#bcbec5`    | Lift-quotes, source lines, tertiary detail.       |
| `--border`   | oklch(92% 0.008 240)   | `#e6e8ed`    | Hairline dividers (1px). Never thicker.           |
| `--accent`   | oklch(48% 0.13 235)    | `#1f6db8`    | The single brand colour. CTAs, link underlines.   |
| `--accent-quiet` | oklch(95% 0.025 235) | `#e8eef7`    | Accent backgrounds (CTA wash). Used twice max.    |

Rules:
- Never use `#000` or `#fff`. Both feel cheap on screen and reek of "AI-generated".
- One accent colour. Never two. The brand has *one* colour and it carries the CTA system AND the structural accent system (bullet markers, stage labels, focused link underlines). The rule is "one accent, used everywhere", not "accent on CTAs only".
- Ratio test: 70% bg, 25% fg, 5% accent. If accent exceeds 5% of pixels, kill some of it.
- Contrast: `--fg` on `--bg` = 12.4:1 (AAA). `--fg-quiet` on `--bg` = 4.7:1 (AA). `--accent` on `--bg` = 5.2:1 (AA).

---

## 3. Typography Rules

ONE typeface across the entire site: **Söhne** (Klim) via the free Söhne Mono Variable swap-in OR — since Söhne is licensed — **Inter Display** as the open fallback. NOT plain Inter (the default LLM-page typeface — soup-rule rat).

Decision for v1 ship: use **Geist** by Vercel (open-source, OFL, downloadable, distinct from Inter, looks editorial). Loaded self-hosted, not from Google Fonts (faster, no tracking, no banner needed).

Scale (mobile-first, fluid via clamp):

| Role          | Size                                  | Weight | Line-height | Tracking        |
|---------------|---------------------------------------|--------|-------------|-----------------|
| H1 (hero)     | clamp(2rem, 5.5vw, 3.5rem)            | 600    | 1.1         | -0.02em         |
| H2 (section)  | clamp(1.5rem, 3.5vw, 2rem)            | 600    | 1.2         | -0.015em        |
| H3 (FAQ Q)    | 1.125rem (18px)                       | 600    | 1.4         | -0.01em         |
| Lead          | clamp(1.125rem, 2.2vw, 1.375rem)      | 400    | 1.55        | -0.005em        |
| Body          | 1.0625rem (17px)                      | 400    | 1.65        | 0               |
| Small         | 0.875rem (14px)                       | 500    | 1.5         | 0               |
| CTA button    | 1rem (16px)                           | 600    | 1           | 0               |

Hierarchy is achieved by **colour + weight first, size second**. Body and lead are barely different in size — the lead is heavier-tracked and on a longer line-height to feel airier. Skim hierarchy is via heading colour (`--fg`) vs body colour (`--fg`) with ghosted intros (`--fg-quiet`).

OpenType: enable `ss01`, `ss02`, `cv01` if Geist exposes them; tabular nums on any number-heavy block.

---

## 4. Component Stylings

### Buttons

```css
.btn-primary {
  display: inline-flex; align-items: center; gap: 0.5rem;
  padding: 0.875rem 1.5rem;
  background: var(--accent);
  color: var(--bg);
  font-size: 1rem; font-weight: 600;
  border: 0; border-radius: 0.5rem;
  text-decoration: none;
  transition: background-color 200ms cubic-bezier(0.4, 0, 0.2, 1);
}
.btn-primary:hover { background: oklch(43% 0.13 235); }   /* darken accent 5L */

.btn-secondary {
  /* ghost text link with arrow, no box. Used for "Or book a 20-min call" */
  color: var(--fg-quiet);
  text-decoration: underline;
  text-underline-offset: 0.2em;
  text-decoration-thickness: 1px;
  text-decoration-color: var(--border);
}
.btn-secondary:hover {
  color: var(--accent);
  text-decoration-color: var(--accent);
}
```

Rule: **one** primary button per section. The page has the same primary button in two places (hero, final CTA) and nowhere else.

### FAQ items

Plain `<details>` / `<summary>` — semantic, accessible, no JS. Hairline border above each, +/- indicator on the right via `::after`. Open state expands the content with a 200ms grid-template-rows transition (no JS animations).

### Inline images

72×72px square (kitchen worktop tile), 56×56 round (avatar). Inline-flex with body text, baseline-aligned. Border-radius 0 for tiles, 50% for avatars. No drop shadows, no captions — context comes from the surrounding sentence. Loaded as `<img>` with `loading="lazy"` and `decoding="async"`, explicit width/height to prevent CLS.

### Footer

Single block, left-aligned, `--fg-quiet`, 14px. Four short lines:
- `Clyde Reply Ltd, Lenzie, East Dunbartonshire`
- `Companies House registration pending · ICO registration pending`
- `hello@clydereply.co.uk`
- `© Clyde Reply 2026`

No social icons, no nav links, no "made with" footer-pride.

---

## 5. Layout Principles

**Single column, max-width 720px, centred.** Wider than reboot.studio's 856 because our copy is denser (sales LP, not portfolio). Content padding 24px L/R on mobile, 40px on tablet+, capped at 720px regardless of viewport.

**Section rhythm**: 96px vertical between sections on desktop (`clamp(64px, 10vw, 96px)` for fluid). The hero gets `padding-top: clamp(80px, 15vh, 160px)` — a deliberate fall-of-air before the H1.

**Spacing scale (8pt)**: `--s-1: 4px; --s-2: 8px; --s-3: 16px; --s-4: 24px; --s-5: 32px; --s-6: 48px; --s-7: 64px; --s-8: 96px`. Use these tokens everywhere; no arbitrary pixel values in the CSS.

**Bullet lists** are vertical, 24px gap between items, no bullet glyph (use `•` inline at the start of each text line, in `--fg-quiet`). Soup law: bullets are aesthetic objects — equal length, parallel rhythm.

**Header**: tiny. Wordmark "Clyde Reply" in `--fg`, 16px / 600 weight, top-left, padding 24px. NO right-side nav. NO menu. Single brand mark.

**Persistent CTA?** No. Reboot has one because it's a portfolio site — visitors browse for minutes. Iain skim-reads in 30 seconds and either clicks the hero CTA or bounces. A persistent CTA on a long-form sales LP feels like a SaaS — wrong register.

---

## 6. Depth & Elevation

Flat by default. The page should feel like a printed page, not an app.

ONE permitted shadow: the primary CTA on hover gets a subtle lift —
```
box-shadow: 0 1px 2px oklch(48% 0.13 235 / 0.15);
```
That's it. No card shadows, no section shadows, no glow.

Borders carry the structural weight: hairline 1px in `--border` between sections where a divider helps (FAQ items). Most sections have no divider — whitespace is the divider.

---

## 7. Do's and Don'ts

**Do:**
- One typeface, one accent colour, one primary CTA, one column.
- OKLCH only. Tint neutrals toward hue 240. No pure #000 or #fff.
- Type-driven hierarchy: colour and weight first, size second.
- Generous whitespace between sections (96px on desktop).
- 17px body, 1.65 line-height. Long enough to be a letter.
- "You" in every paragraph. The reader is the protagonist.
- Bullets parallel and equal-length. Visual rhythm matters.
- Mobile-first sentences ≤ 3 lines on iPhone 13 Pro.
- Lazy-load images, explicit width/height to prevent CLS.

**Don't:**
- Inter. Use Geist (or Söhne if licensed).
- Drop shadows on cards.
- Gradient backgrounds, especially purple→pink.
- Bouncy/elastic easing. Use `cubic-bezier(0.4, 0, 0.2, 1)` only.
- Pure black or pure white anywhere.
- More than one accent colour. Ever.
- Stock photos of laptops, AI-brain icons, glowing spheres.
- Live-chat widget. Newsletter pop-up. Cookie banner (no cookies → no banner).
- "Premium", "innovative", "world-class", "leverage", "synergy", "transform". Soup-rats.
- Stat blocks ("10x ROI"). Iain doesn't believe them.
- Testimonial-shaped boxes when we have no testimonials.

---

## 8. Responsive Behavior

**Breakpoints**: 0–479 (mobile portrait), 480–767 (mobile landscape / small tablet), 768–1023 (tablet / small laptop), 1024+ (desktop). Mobile-first cascade.

**Touch targets**: minimum 44×44px for all clickable elements (impeccable rule). The primary CTA is 48px tall to err on the safe side.

**Type scaling**: H1 fluid via `clamp(2rem, 5.5vw, 3.5rem)` — never larger than 56px (3.5rem) even on a 4K monitor. Body stays at 17px regardless of viewport (above 720px content width is centred not stretched).

**Hero image**: `aspect-ratio: 4/3` on mobile, `16/9` on tablet+. Lazy-loaded. `<picture>` with WebP primary, JPG fallback. Width attribute set to 720 (the max content column).

**Bullets**: vertical on all viewports. No horizontal collapse.

**FAQ**: full-width on mobile, same 720px column on desktop. The summary toggle indicator is 24×24 px and aligned right.

---

## 9. Agent Prompt Guide

When an LLM is asked to add a new section, modify copy, or render the HTML for this site, it MUST:

- Read this DESIGN.md first.
- Use only the colour tokens defined in Section 2. No hex literals in markup.
- Use only the spacing tokens defined in Section 5. No arbitrary padding/margin values.
- Use Geist for all text. No fallback to Inter.
- Apply the Soup voice rules (`/Users/foxy/.claude/plugins/soup/references/voice-rules.md`). "You" in every sentence. No fluffy adjectives. ≤3-line mobile sentences. Bullets as aesthetic objects.
- Keep ONE accent colour. ONE primary CTA per section.
- No JavaScript unless necessary for an interaction (FAQ toggle is HTML/CSS only via `<details>`).
- Use semantic HTML5 (`<header>`, `<main>`, `<section>`, `<footer>`, `<details>`, `<summary>`, `<address>`).
- Set explicit width/height on every image.
- Set `loading="lazy" decoding="async"` on images below the fold.
- Set `lang="en-GB"` on `<html>`.
- Set OpenGraph + Twitter card meta tags.

Reference check before shipping any change:
- [ ] Does the new copy pass the soup voice rules?
- [ ] Is the headline-only sequence still coherent (Dual Readership Path)?
- [ ] Does mobile sentence wrap stay ≤ 3 lines on iPhone 13 Pro (375px)?
- [ ] Is there exactly one accent colour, one primary CTA per section?
- [ ] Does every fact have a "so you can…" attached?

If any check fails, fix before shipping.

---

## Notes for impeccable's anti-slop detector

When `npx impeccable detect` is run on this site (post-deploy), the
expected pass criteria:

- No Inter (Geist instead).
- No `#000`/`#fff` literals.
- No purple/pink gradients.
- No bouncy easing (only `cubic-bezier(0.4, 0, 0.2, 1)`).
- All touch targets ≥ 44px.
- All headings in semantic order (no skipped levels).
- Line length 60–75ch on body.
- Section padding ≥ 64px on desktop.
- No drop shadows on cards.
- No gray text on coloured backgrounds.

If the detector flags anything else, fix it. Don't argue with the detector.
