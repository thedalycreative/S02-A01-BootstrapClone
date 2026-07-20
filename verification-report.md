# Verification Report — Bootstrap 5 Rebuild vs. Live Site

**Live URL:** https://hpforbiddenforestexperience.com/
**Verified:** 2026-07-21, via headless Chromium (Playwright MCP), viewport 1080×720 CSS px.
**Method:** computed styles read with `getComputedStyle` on the live page and on the local build
(served at `http://127.0.0.1:8642/`), full-page screenshots captured for both
(`live-fullpage.jpeg`, `clone-fullpage.jpeg` at repo root).

---

## 5A — HTML structure verification

| # | Check | Live | Built | Match |
|---|-------|------|-------|-------|
| 1 | Section census & DOM order | header-lang-bar → hero → intro → milestones → city-selector → finished-experiences → press → sorting-hat → firefly-effect → footer (+ cookie overlay) | identical order, same section ids (`intro`, `milestones`, `city-selector`, `finished-experiences`, `press`, `sorting-hat`, `firefly-effect`, `footer`) | PASS |
| 2 | City tiles total | 16 (`.city`) | 16 (1 available + 15 finished) | PASS |
| 3 | Available / Finished split | 1 / 15 | 1 / 15 | PASS |
| 4 | Press slides | 6 (`#press .swiper-slide`) | 6 (`figure.hp-press__card`) | PASS |
| 5 | Milestone stat items | 3 | 3 | PASS |
| 6 | Fireflies | 9 (`.firefly`) | 9 | PASS |
| 7 | Footer nav links | 5 (Contact, Influencers, Press, Affiliates, Licensing) | 5, same labels + hrefs | PASS |
| 8 | Partner logos | 2 (WB, Fever) | 2 | PASS |
| 9 | Social icons | 2 (Facebook, Instagram) | 2 | PASS |
| 10 | Hero icon trio | Interactive / Family friendly / Outdoor Trail | same 3, same SVGs | PASS |
| 11 | Carousel vs static grid | press = Swiper carousel (1/2/3 per view by breakpoint) | Bootstrap carousel, 2 slides × 3 cards | PASS (accepted deviation: grouping, see below) |
| 12 | Ribbon/chip position | status chip overlays tile top-left, outside-in | same (absolute, left:0, margin 2rem 0) | PASS |
| 13 | Semantics | header/nav/section/footer | same + `figure/figcaption` for press quotes | PASS |
| 14 | Content parity | headings & key copy | identical headings/copy; press quotes paraphrased (see deviations) | PASS |

## 5B — Styling verification (computed values)

| Property | LIVE value | BUILT value | Match |
|----------|-----------|-------------|-------|
| Body font stack | `Primary, Secondary` (Helvetica Neue woff2) | `HelveticaNeueW` (same woff2, self-hosted) | PASS |
| Page background | `#020a17` | `#020a17` (`--hp-bg-dark`) | PASS |
| Intro h1 font | `Secondary, serif` 52px/56px, `#a3e6ef` | `HarryBeast` 52px/56px, `#a3e6ef` | PASS |
| Hero tagline | `Secondary` 48px, weight 100, `#fff` | same | PASS |
| Primary button | bg `rgb(189,158,99)`, text `#fff`, radius 24px, 14px/600, ls 1px, uppercase, min-h 48px, pad-x 24px | identical | PASS |
| Button hover | `filter: grayscale(.5)` | identical | PASS |
| City img ring | radius 32px, border 1px `rgb(163,230,239)` | identical | PASS |
| Chip — Available | bg `rgb(56,195,158)`, radius 0 4px 4px 0, 14px, pad 4px 8px, margin 32px 0, shadow `0 1px 3px #00000063` | identical | PASS (fixed: was palette default `#18824c`) |
| Chip — Finished | bg `rgb(182,62,104)` | identical | PASS (fixed: was `#d10047`) |
| Stat number | `Secondary` 40px `#a3e6ef` | identical | PASS |
| Stat card | border 1px `#a3e6ef`, radius 32px, padding 16px, icon 90×90 | identical | PASS (fixed: was borderless) |
| Milestones bg | `linear-gradient(0deg, rgba(16,24,49,1) 0%, rgba(2,10,23,1) 100%)` | identical | PASS |
| City section bg | `rgb(16,24,49)` | identical | PASS |
| Press section bg | `linear-gradient(0deg, rgba(2,10,23,1) 0%, rgba(16,24,49,1) 100%)` | identical | PASS |
| Press card | bg `#a3e6ef`, radius 32px, padding 32px 40px, text `rgb(2,10,23)` 20px/24px | identical | PASS (fixed: was borderless dark) |
| Footer bg | `#020a17` + treeline webp, top/cover/no-repeat | identical (local copy of same webp) | PASS |
| Footer nav link | `rgb(229,219,193)` | identical | PASS (fixed: was white) |
| Copyright bar | bg `rgb(229,219,193)`, black text, 11px | identical | PASS |
| Intro background | fixed-attachment starfield (remote CDN webp) + masked moon (`linear-gradient(0deg,transparent 10%,black 20%,black 80%,transparent)` mask), moon 300×300 fixed, `opacity(.8)` | identical, assets localized | PASS |
| Hero bottom fade | `linear-gradient(180deg,#0000 76.48%,#020a17)` | identical | PASS |
| Container cap | max-width 75rem | `.hp-container-cap` 75rem | PASS |
| Responsive city grid | 47% → 30% (≈900px) → 20% (≈1200px) per tile | `row-cols-2` → `row-cols-md-3` → `row-cols-xl-5` | PASS (fixed: 5-col was firing at ≥992) |

## 5C — Asset location verification

| Check | Result |
|-------|--------|
| Every `src`/`href`/`poster`/CSS `url()` resolves on disk | PASS — 41 unique local refs, 0 missing (scripted check) |
| `style="` occurrences in index.html | PASS — 0 |
| Marker pairs | PASS — 24 ⬇ marks / 24 ⬆ marks, one pair per section ×12 |
| Filename `-WxH` dims equal actual pixel dims | PASS — scripted with `sips`, all match; SVG dims from viewBox |
| Assets in correct section folders (`images/`/`videos/` split) | PASS — shared logos/textures/favicons/fonts in `00-universal/`, hero posters in `02-hero/videos/` with `source-url.txt` files |
| Nothing referenced lives in `_raw/` | PASS |
| Remote video srcs return HTTP 200-class | PASS — desktop webm/mp4 + mobile mp4 all return `206 Partial Content` to range requests |
| Images used in same location as original | PASS — spot-checked hero poster, city tiles, press logos, partner logos against live page |
| Zero console 404s on the build | PASS — only transient `ERR_CONNECTION_RESET` from the single-threaded dev server under load, not asset errors; clean on reload |

## Fixes applied during this phase

1. Primary button text `#000` → `#fff`, weight 700 → 600 (live theme overrides palette default).
2. "Available" chip `#18824c` → `#38c39e`; "Finished" chip `#d10047` → `#b63e68`.
3. Press quotes restyled from bare dark-background text to ice-blue cards (`#a3e6ef`, radius 32px, padding 32px 40px, dark navy text).
4. Milestone stats wrapped in bordered cards (1px `#a3e6ef`, radius 32px, padding 16px) with 90px icons.
5. Intro h1 3.25rem breakpoint moved from ≥1200px to ≥900px (live computes 52px at 1080w).
6. Footer nav links white → parchment `#e5dbc1`.
7. City grid 5-column breakpoint moved from `lg` (992px) to `xl` (1200px).
8. Hero tagline weight 400 → 100.

## Accepted deviations (with reasons)

| Deviation | Reason |
|-----------|--------|
| Press carousel groups 3 cards per slide (2 slides) instead of Swiper's per-card loop with partial peek | Rebuilt with stock Bootstrap 5 carousel per the Bootstrap-first rule; card count (6), card styling and dot indicators match. |
| Press quotes are paraphrased summaries, not verbatim excerpts | The verbatim blurbs are the outlets' copyrighted text; summaries preserve layout and meaning for private practice. |
| OneTrust cookie widget replaced by a static dismissible Bootstrap alert | Third-party consent SaaS cannot run locally; visual parity kept (dark bar, parchment-outline button). |
| Sorting-hat / language-selector / footer links point at the live external URLs | Same targets as the original nav. |
| Firefly drift paths use 3 authored `@keyframes` variants instead of the site's 15 (`move1`–`move15`) | Visual effect is equivalent; 9 firefly elements and the `drift`/`flash` glow mechanic are faithful. |
| Hero `<video>` uses the two 16:9 desktop sources; the 9:16 mobile file is referenced via poster + `source-url.txt` | HTML5 ignores `media` attributes on `<video><source>`; the original site swaps sources with custom JS. Mobile poster still swaps via `<picture>`. |

All rows PASS as of the fixes above. Screenshot pair reviewed side-by-side: `live-fullpage.jpeg` vs `clone-fullpage.jpeg`.
