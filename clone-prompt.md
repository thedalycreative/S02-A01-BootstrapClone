# 🪄 clone-prompt.md — Saved Webpage → Organised Bootstrap Clone

> **AGENT: THIS FILE IS YOUR TASK.** You are an expert automation engineer and
> senior frontend developer. This folder contains a webpage saved from a
> browser (Chrome "Webpage, Complete"). Execute every phase below, in order,
> without asking for permission between phases. Do not stop until the
> Verification Checklist at the bottom fully passes. Work entirely inside this
> folder.

---

## 🎯 Mission
Transform this raw browser save into ONE polished deliverable: a faithful
**Bootstrap 5 rebuild of the site** with every image and asset from the save
organised into a clean numbered structure, plus a **comprehensive brand kit /
styleguide** and a **README** rich in visuals documenting the fonts, colours,
and styling pulled from the original site. Visual fidelity to the original is
the #1 success criterion.

---

## 🧭 PHASE 0 — Detect What You Were Given
1. Inventory this folder. Identify the saved HTML file and the browser's
   asset dump (`SiteName_files/` or similar).
2. Recover the **original URL** from the HTML: `<link rel="canonical">`,
   `<meta property="og:url">`, or absolute links. Record it — needed for
   remote video srcs, deeper style sampling, and the README.
3. If the save is incomplete: re-fetch the live page and any missing linked
   CSS/JS/images/fonts from the recovered URL with a browser User-Agent.
   **Always fetch the site's full CSS files** even if the save has inlined
   styles — the complete stylesheets are the source of truth for Phase 1
   style sampling.
4. Create `_raw/` and move the original save files into it untouched.
   **Never delete the user's originals** — `_raw/` is the archive; the user
   removes it themselves when happy.

---

## ⚖️ GOLDEN RULES — never violate any of these
1. **VERIFY, DON'T GUESS.** Read the saved/fetched CSS for real values: exact
   hex colours (build a full frequency table, including gradients, hover
   states, border and shadow colours — not just the top 5), `border-radius`,
   `box-shadow`/glows, padding/margins, letter-spacing, font weights and
   sizes per element, `@keyframes`. Pill buttons stay pill. A `#d5ac5c`
   ribbon stays `#d5ac5c` and stays in its true DOM position (above vs below
   the navbar — check, don't assume). If you can render screenshots, diff
   your build against the original before finishing.
2. **REAL ASSETS ALWAYS.** Every image in the save gets used where the
   original uses it. Placeholders exist ONLY for media that is genuinely
   unobtainable (auth-gated embeds, DRM streams) — and each use must be
   listed in the README's Manual Follow-ups.
3. **VIDEOS: REMOTE SRC + LOCAL POSTER.** Find every video: `<video>`,
   `<source>`, `data-src`, poster attributes, and `.mp4`/`.webm`/`.m3u8`
   URLs referenced inside the saved JS bundles (grep them BEFORE
   discarding). Do not bundle video files. Embed with the absolute remote
   URL and a local poster:
   `<video src="https://cdn…/hero.mp4" poster="assets/02-hero/videos/02-hero-video-poster-1080x608.webp" autoplay muted loop playsinline></video>`
   Save each poster into the section's `videos/` folder next to a
   `<name>-source-url.txt` containing the remote URL. Log all video URLs in
   the README Media Sources table. A dead CDN link must degrade to the
   poster, never a black box.
4. **00-universal/ = anything shared or global**: favicon set, EVERY logo
   variant (light/dark/stacked — suffix them), dividers/flourishes used as
   brand ornaments, og/share image, and full-page or section background
   textures (night skies, treelines, starfields). Section folders hold only
   assets unique to that one section.
5. **DEV TOC IS A COMMENT, NOT UI.** The development Table of Contents sits
   at the very top of `<body>` inside `<!-- -->`. It renders nothing and is
   never mixed into the navbar markup.
6. **COMPONENT PARITY, COUNTED.** Count repeated components on the original
   (cards, tiles, reviews, offers, carousel slides) and reproduce the same
   count, grid columns, and media type. 6 offer cards = 6 cards. A review
   carousel = a Bootstrap carousel, not a static grid. Animated section
   backgrounds get a CSS `@keyframes` or fixed-attachment equivalent.
7. **EMBEDS STAY EMBEDS.** Maps become a live iframe:
   `https://www.google.com/maps?q=<url-encoded address>&output=embed` —
   never a placeholder image. Same for any other keyless embed.
8. **BOOTSTRAP-FIRST STYLING.** Use Bootstrap 5 utilities before custom CSS:
   `ratio`, `object-fit-cover`, `rounded-*`, `shadow`, spacing
   (`p-* m-* g-*`), flex/display utilities, `sticky-top`, carousel,
   accordion. `css/styles.css` is the BRAND layer only: colour variables,
   font bindings, glows Bootstrap can't express, gradients, keyframe
   animations. Audit at the end: any custom rule duplicating an existing
   Bootstrap utility gets replaced.
9. **NO INVENTED NUMBERS.** Every dimension on a diagram and every value in
   the README is measured from the real CSS/images. A diagram or swatch with
   made-up values is worse than none.

---

## 🗂️ PHASE 1 — Analyze
1. Map the page's structural sections (`header/nav/section/footer`) in DOM
   order. Number them: `00` head/universal, `01` navbar, `02` hero, … through
   the footer. These numbers drive folders, CSS blocks, and HTML markers.
2. Inventory ALL media across the saved HTML, asset folder, CSS files
   (`background-image` urls), and JS bundles: images, videos+posters, fonts.
3. **Deep style sampling** (this feeds the brand kit — be thorough):
   - Full colour frequency table with role assignment: backgrounds, text,
     headings, accents, buttons, borders, shadows, gradient stops, hover
     states, overlay tints (with their opacity values)
   - Typography audit: every font family in use, mapped element-by-element —
     h1–h6, body, nav links, buttons, captions — with size, weight,
     letter-spacing, line-height, and text-transform for each. Map
     self-hosted fonts to the nearest Google Font and note the swap.
   - Component DNA: button radius/padding/transform/hover, card/tile
     radius + padding + border + shadow, container max-widths, section
     vertical rhythm
   - Effects: every `box-shadow`/glow verbatim, gradients verbatim, named
     `@keyframes` worth keeping (twinkle, float, pan)
4. Extract each section's real text content for a faithful rebuild.

## 🖼️ PHASE 2 — Assets
Build this exact tree:
```
assets/
├── styleguide/
│   ├── logos/         all logo variants
│   ├── badges/        tech badges for the README (HTML5, CSS3, Bootstrap 5)
│   ├── diagrams/      4–6 key-technique annotated SVGs (see Phase 5)
│   ├── colors.svg     generated FULL palette board (see Phase 5)
│   ├── fonts.svg      generated typography specimen board (see Phase 5)
│   └── components.svg generated component DNA card (buttons, tiles, radii)
├── 00-universal/
│   ├── images/        favicon set, dividers, bg textures, og image
│   └── videos/        posters + source-url.txt for global/hero videos
├── 01-…/ … NN-footer/
│   ├── images/        section-unique images
│   └── videos/        posters + source-url.txt (only where videos exist)
└── placeholders/      colour-coded fallback SVGs (Image=black/grey,
                       Photo=yellow, Video=red/white, Logo=white/dark;
                       landscape/portrait/square/banner/thumb, X-cross +
                       centred dimension labels) — for future/missing media
```
1. Dedupe responsive variants of the same asset — keep the largest.
2. Rename everything: `[SectionNum]-[Section]-[ShortName]-[WxH].[ext]`
   (SVG dims from viewBox; omit dims rather than fake them).
3. Move remaining raw leftovers (tracking snippets, production JS, the
   browser's `_files` dump) into `_raw/` once nothing referenced remains
   outside it.

## 🎨 PHASE 3 — css/styles.css
Single stylesheet, in this order:
1. `/* CSS TABLE OF CONTENTS */` listing every section block.
2. `SECTION 00: UNIVERSAL / ROOT` — `:root` variables (ALL sampled colours
   with RGB + usage comments, not just five), Google Font imports and
   bindings, reset, full type scale matching the sampled audit, brand button
   classes matching sampled radius/shadow EXACTLY, global
   background/animation helpers.
3. One fenced block per section (`/* SECTION 01: NAVBAR STYLING */` …) with
   that section's brand styling: glows, gradients, hover states, keyframes,
   responsive tweaks Bootstrap utilities can't cover.

## 🧱 PHASE 4 — index.html
1. Wrap the head and every section in highly visible marker pairs:
```
<!-------------------⬇ --- SECTION 00: HEAD TAG DATA   -- ⬇------------------->
<head>
<!--TITLE--> <title>…</title>
<!--ICON--> <link rel="icon" href="assets/00-universal/images/…">
<!--KEYWORDS--> <meta name="keywords" content="…">
<!--STYLESHEETS-->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
<link rel="stylesheet" href="css/styles.css">
</head>
<!------------------- ⬆ --- SECTION 00: HEAD TAG DATA   -- ⬆ ------------------->
```
   Apply sequentially to every section; ids must match the dev TOC anchors.
2. Bootstrap 5 CSS + JS bundle via CDN (verify URLs resolve).
3. Dev TOC as a pure HTML comment at the top of `<body>` (Golden Rule 5).
4. Faithful rebuild: real copy, real images, remote-src videos with local
   posters, correct component counts, carousels where the site has them,
   live map iframe, announcement ribbon in its true position and true
   colour.
5. Zero inline `style=` attributes. All local paths must resolve on disk;
   all remote video srcs must return HTTP 200.

## 🔍 PHASE 5 — VERIFY AGAINST THE LIVE SITE (before any README is written)
This phase produces `verification-report.md` — the measured, confirmed data
that the README is built FROM. The README must never contain a dimension,
colour, or value that does not appear in this report. Fetch the live page
fresh (recovered URL, browser User-Agent). Use a headless browser for
computed styles and screenshots if one is available; otherwise verify against
the fetched source HTML + full CSS files.

### 5A — HTML structure verification
Compare your built `index.html` against the live page and record PASS/FAIL
per item, fixing every FAIL before moving on:
1. **Section census:** same sections, same DOM order, same ids/roles.
2. **Component counts:** for every repeated component (cards, tiles, review
   slides, offer cards, nav links, footer partner logos) — live count vs
   built count. Numbers must match exactly.
3. **Positional truth:** announcement ribbon above/below nav, sticky
   behaviours, carousel vs static grid, section background layers — as
   rendered live, not as assumed.
4. **Content parity:** headings and key copy per section match the live text
   (allowing minor trims noted in the report).
5. **Semantics:** header/nav/section/footer/figure/address used where the
   meaning demands them.

### 5B — Styling verification (measure, compare, fix, record)
For each item, record three columns in the report — LIVE value, BUILT value,
MATCH? — and fix the build until every row matches:
1. **Colours:** background of every section, heading colour, body colour,
   button fill + border + hover, ribbon colour, link colours, gradient stops
   with their exact positions, overlay tints with opacity.
2. **Typography:** for h1, h2, h3, body, nav link, button — family, size,
   weight, letter-spacing, line-height, text-transform (computed values
   where possible; the site's CSS declarations otherwise).
3. **Component DNA:** button border-radius + padding, card/tile radius +
   padding + border + box-shadow (verbatim shadow strings), container
   max-widths, section vertical padding.
4. **Effects:** every glow/box-shadow, every gradient, every @keyframes
   replicated — name them in the report.
5. **Responsive:** at 1440px and 390px widths, grid columns per section
   collapse the same way as the live site (screenshot both if possible).

### 5C — Asset location verification
1. Every `src`/`href`/`poster`/CSS `url()` in the build resolves on disk —
   zero 404s (script it; open the page and check the console if a browser is
   available).
2. Every asset sits in the CORRECT section folder and correct `images/` or
   `videos/` subfolder; shared/global assets are in `00-universal/`; nothing
   referenced still lives in `_raw/`.
3. Filename dimensions are truthful: the `-WxH` in each filename equals the
   file's actual pixel dimensions (script-check with an image library).
4. Each image is used in the SAME location the original uses it (hero image
   in hero, tile images in their tiles) — spot-check against the live page.
5. Remote video srcs return HTTP 200 and each has its local poster +
   `source-url.txt`.

### 5D — The report
Write `verification-report.md` at the folder root containing: the three
PASS/FAIL tables above with all measured values, screenshots side-by-side if
captured, a list of every fix applied during this phase, and any accepted
deviations with reasons. **Do not proceed to Phase 6 until every row is
PASS.** The README's dimensions, colours, typography metrics, and diagram
numbers must be copied from this report — never re-measured from memory.

## 📖 PHASE 6 — README.md (visual-heavy build documentation)
Write in first-person-plural voice — "Hey! This is our Bootstrap 5 rebuild of
<Site Name> (<original URL>). Here's how it's put together." The README must
be VISUAL: generated SVG boards embedded inline, not walls of text.
**Source of truth: every number, hex, and metric comes from
`verification-report.md` (Phase 5). If a value isn't in the report, verify it
and add it to the report first.** Required structure:

1. **Hero header** — title, one-line vibe, tech badges from
   `assets/styleguide/badges/`, Live Preview tip (`Ctrl/Cmd + Shift + V`),
   link to the original site.

2. **🛠️ How This Was Made** — short numbered build journal: saved the page →
   ran clone-prompt.md → assets organised → styles sampled from the real
   CSS → rebuilt with Bootstrap → verified. Name tools/links used.

3. **🚀 Running It** — open `index.html` in a browser or Live Server; folder
   map; where the styleguide lives.

4. **🎨 Colour System (visual-first)** — generate and embed
   `assets/styleguide/colors.svg`: a full palette board, one row per colour —
   large swatch, variable name, hex, RGB, and a usage caption naming actual
   elements ("hero overlay @ 85% opacity", "button hover border"). Include
   EVERY sampled colour role: backgrounds, headings, body text, accents,
   buttons + hover states, borders, shadows/glows, and a rendered strip for
   each gradient with its stop values. Below the board: the same data as a
   markdown table, plus links to colour tools
   ([imagecolorpicker.com](https://imagecolorpicker.com),
   [htmlcolorcodes.com](https://htmlcolorcodes.com),
   [W3Schools colour picker](https://www.w3schools.com/colors/colors_picker.asp)).

5. **🔤 Typography (visual-first)** — generate and embed
   `assets/styleguide/fonts.svg`: a specimen board rendering each font at its
   real sizes — h1 through h6, body, nav link, button label — each annotated
   with family, weight, size, letter-spacing, line-height, and
   text-transform as sampled from the site. Note any self-hosted → Google
   Font swaps with links to each Google Fonts page. Below the board: the
   element-by-element table.

6. **🧬 Component DNA (visual-first)** — generate and embed
   `assets/styleguide/components.svg`: rendered examples of the site's
   button (normal + hover), card/tile, and input styles with their measured
   radius, padding, border, and shadow values labelled on the drawing.

7. **🧩 Section-by-Section Teardown** — one block per section:
   - **Bootstrap element used** with direct links to that component on
     getbootstrap.com + the matching MDN and W3Schools pages
   - **Layout numbers**: margin, padding, container behaviour, col classes
     per breakpoint
   - **The trick**: short fenced HTML+CSS snippet showing the one technique
     that makes it work (video background layering, `ratio` +
     `object-fit-cover` tile sizing, mobile text stacking…)
   - **Diagram**: embed the relevant key-technique SVG from
     `assets/styleguide/diagrams/` (or link the closest)
   - **Deeper reading**: only the flexbox/grid/positioning links genuinely
     needed for THIS section
   Key-technique diagrams (4–6 total): hero video layer stack, card/tile
   anatomy with px dimensions, responsive grid stacking across breakpoints,
   navbar structure, footer structure. Measured values only.

8. **📁 Section Number Key** table + placeholder legend.

9. **🔮 Tone & Branding** — the site's vibe in evocative language; Do's
   (mobile-first grids, contrast, semantic markup) / Don'ts (no inline
   styles, no images without alt text).

10. **📌 Media Sources & Manual Follow-ups** — table of every remote video
    URL, anything unobtainable, and exactly what to source. Add a note that
    the original site's imagery and branding belong to its owner and this
    rebuild is for private practice only.

---

## ✅ VERIFICATION CHECKLIST — all must pass before you finish
- [ ] Final folder tree matches the Output Contract below
- [ ] Every local path in index.html + styles.css resolves on disk
- [ ] Remote video srcs return HTTP 200; each has a local poster + source-url.txt
- [ ] `style="` occurrences in index.html: **0**
- [ ] ⬇ count == ⬆ count, one pair per section
- [ ] Dev TOC renders nothing (pure comment)
- [ ] NO placeholder used where a real saved asset exists
- [ ] Repeated-component counts match the original site
- [ ] Button radius, ribbon colour+position, glows match sampled values
- [ ] Map is a live iframe, not an image
- [ ] `00-universal/` holds favicon set, ALL logo variants, dividers, bg textures
- [ ] Every section folder uses `images/` and `videos/` subfolders correctly
- [ ] styleguide/ has colors.svg (full palette incl. gradients + hovers),
      fonts.svg (element-by-element specimen), components.svg, badges,
      and 4–6 measured diagrams
- [ ] README embeds the three styleguide boards inline and reads as a
      visual build document, not a spec sheet
- [ ] No custom CSS duplicating an existing Bootstrap utility
- [ ] Originals preserved untouched in `_raw/`
- [ ] Phase 5 completed BEFORE the README: verification-report.md exists at
      root with all 5A/5B/5C tables fully PASS, fixes logged
- [ ] Every dimension/colour/metric in the README traces to
      verification-report.md
- [ ] Screenshot diff vs original reviewed (when a browser is available)

## 📦 Output Contract
```
<this folder>/
├── clone-prompt.md        ← this file (leave in place)
├── index.html             ← the finished Bootstrap 5 clone
├── css/styles.css         ← brand-layer stylesheet
├── README.md              ← visual build documentation
├── verification-report.md ← Phase 5 live-site comparison (README's source)
├── assets/                ← organised tree per Phase 2
└── _raw/                  ← untouched original save (user deletes later)
```
Finish with a concise run summary: sections found, assets kept vs deduped,
colours and fonts sampled, videos wired, diagrams generated, and anything
listed in Manual Follow-ups.
