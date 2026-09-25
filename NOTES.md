# Osmo (osmo.supply) · Clone Notes

## Source info

- Original URL: https://www.osmo.supply/
- Source repository: none found. GitHub search (`osmo supply in:name,description`) returned only
  third-party fan recreations (e.g. `devdignesh/osmo`, `user1738mike/oslo`) — no official source.
- Original author: Osmo — Dennis Snellenberg & Ilja van Eck (per site JSON-LD).
- Licence: **NONE / proprietary.** The site is a commercial Webflow build. Code, copy, fonts and
  media are Osmo's. This clone is for local study and further remixing only —
  **do not deploy it publicly without replacing brand, copy, fonts and media.**
- Attribution: keep the Osmo credit and the founder links while it stays a study artifact.

## Tech stack

- **Webflow** static export — `data-wf-site="68a5787bba0829184628bd51"`, page
  `data-wf-page="68a57a9adcd7f453925e8592"`, published **2026-09-17**.
- Custom JavaScript authored in **Slater** (`slater.app/16596/45154,45156,45446,45511.css`,
  `55554.js` + lazily imported `56625.js`).
- **GSAP 3.15** (`gsap`, `Draggable`, `InertiaPlugin`, `ScrollTrigger`, `SplitText`, `Observer`)
  - `CustomEase` 3.14 — every scroll/hover/cursor animation.
- **Lenis 1.3.23** smooth scroll, **Barba 2.10.3** + prefetch page transitions.
- **hls.js 1.6.11** for the bunny.net HLS showcase players, **jQuery 3.5.1** (Webflow dependency).
- Video hosted on **bunny.net** (`osmo.b-cdn.net`), images on `osmo.b-cdn.net` + Webflow CDN.

### Design tokens (read verbatim from the real CSS `:root`)

| Token | Value |
|---|---|
| `--color-neutral-200` (page bg) | `#f4f4f4` |
| `--color-neutral-800` (fg) | `#201d1d` |
| `--color-neutral-900` | `#151313` |
| `--color-purple` | `#6840ff` |
| `--color-coral` | `#f84131` |
| `--color-electric` | `#a1ff62` |
| nav bar height | `4.625em` |
| container padding | `1.875em` |

`body { background:#f4f4f4; color:#201d1d; font-family:"Haffer VF", Arial, sans-serif;
font-size:14px; line-height:20px; letter-spacing:-.01em; font-variation-settings:"wght" 460 }`

### Fonts (self-hosted, real files)

`Haffer VF` (variable), `Haffer XH`, `Haffer Mono` (Regular + Medium), `Brisa Pro` — 11 files in
`assets/fonts/`, wired through the rewritten `@font-face` rules in `assets/css/`.

## Pre-clone assessment

- Complexity: **L4** (animation-heavy brand site: GSAP + Lenis + Barba transitions + HLS media).
  Not L5 — there is no WebGL/Canvas renderer.
- Mode: **Faithful clone.** The site is a Webflow static export, so the *real* HTML/CSS/JS were
  mirrored 1:1 rather than reconstructed.
- High fidelity: markup, section order, CSS, typography, colours, all interactive JS, real
  imagery/video/fonts.
- Approximated: HLS streams are localised at the lowest bitrate variant only.
- Not cloned: accounts, billing, checkout, the gated Vault, CMS-driven inner pages, form POST
  endpoints, analytics.
- Main risk: **licensing.** Everything here is Osmo's proprietary content.

## Run it

```bash
# from the project root — must be served over http, not file://
python -m http.server 8123
# then open http://127.0.0.1:8123/index.html
```

## What changed vs. the original

- **Removed tracking:** Plausible analytics (`plausible.io/js/script.outbound-links.pageview-props.revenue.tagged-events.js`
  + its inline queue stub) and the **Outseta** membership/billing SDK (`cdn.outseta.com/outseta.min.js`,
  its `o_options` config block, and the `Outseta.on('signup')` revenue-tracking handler).
- **Added `assets/js/outseta-stub.js`** — a no-op local stub exposing the exact surface the site's
  own scripts touch (`window.Outseta`, `window.__outseta`, `window.plausible`). Without it,
  `window.__outseta.profile` throws inside the Barba hooks and breaks every later animation.
  It performs no network requests and stores nothing.
- **Localised every asset.** 732 references rewritten to project-relative paths:
  434 images, 376 media files (incl. all HLS segments), 11 fonts, 5 CSS, 16 JS.
- **Removed** `rel="preconnect"` / `rel="canonical"` links and `integrity`/`crossorigin` attributes.
- **Localised the dynamic ESM import** `https://slater.app/16596/56625.js` → `./assets/js/button-pack.js`.
- **Replaced** Webflow's origin-protected `plugins/Basic/assets/placeholder.svg` (403) with a neutral
  local `assets/img/placeholder.svg`. It is only a pre-load filler — JS swaps in the real local image.
- Fixed one URL my harvester had truncated: a 404-page GIF with parentheses in its filename.

## Original vs. clone

| Area | Original | Clone | Trade-off | Evidence |
|---|---|---|---|---|
| Hero | "Dev Toolkit / Built to Flex" with GSAP intro | identical markup + GSAP timeline | none | `RECON/screenshots/clone-desktop.png`, `index.html` |
| Nav / mega-menu | Webflow nav + resource sitemap hover previews | same, driven by local `sm-res-img`/`sm-res-vid` | none | `assets/js/2b39f7-55554.js` |
| Core motion | GSAP + ScrollTrigger + Lenis + Barba + custom cursor | byte-identical libraries, local | none (code path verified, not click-tested) | `assets/js/ddaa8a-gsap.min.js`, `3873b5-lenis.min.js` |
| Media | bunny.net mp4 hover videos + HLS showcase reels | all mp4 local; HLS local @ lowest variant | showcase reels are 480p | `assets/media/` |
| Showcase | 7 "Made with Osmo" HLS players | local HLS playlists + real thumbnails | 480p variant only | `assets/media/*-playlist.m3u8` |
| Mobile | Webflow responsive breakpoints | CSS preserved 1:1 | not visually verified (no browser) | `assets/css/4dd1ac-*.css` |
| Accounts / billing | Outseta gated Vault, checkout | stub — logged-out state | out of scope | `assets/js/outseta-stub.js` |

## Fidelity score

- Source evidence: **5/5** — real Webflow source mirrored; every claim is backed by a local file.
- Structure: **5/5** — original `index.html` DOM kept intact.
- Visual: **5/5** — original CSS, real self-hosted fonts, real colour tokens, real imagery.
- Motion/interaction: **4/5** — all libraries and code paths local, but interactions were not
  exercise-tested in a live browser (none available in this environment).
- Responsive: **3/5** — breakpoint CSS is untouched, but only 1440px was rendered and checked.
- Functional completeness: **4/5** — nav + media + local run work; offline form POSTs and the
  gated template download cannot work.
- Content replacement: **1/5** — deliberately none; this is a faithful clone of Osmo's own content.
- Legal/deployment risk: **2/5** — risk is documented and clear, but genuinely high (proprietary).
- Overall: **Strong faithful reproduction; not deployable as-is.**

## Replacement map (to make it yours)

- Copy → `index.html` (search the hero, `section` blocks, `footer`).
- Imagery → `assets/img/`; video → `assets/media/`.
- Colours → the `:root` block at the top of `assets/css/4dd1ac-osmo-v2.shared.b1655ef4a.min.css`.
- Fonts → `assets/fonts/`; `@font-face` + `font-family` in the same CSS.
- Brand marks → `assets/img/*osmo-logo*`, `*osmo-icon-*`; run `tools/mirror.mjs` again after
  editing source, or edit `index.html` directly.
- **Must replace before any public deploy:** brand name/logo, all copy, the founder credits, the
  `osmo-secure.b-cdn.net` "Lifetime Template" download links, and the `osmo.supply` nav links.

## Verification

- [x] Static: 732/732 local references resolve; CSS refs 0 missing; HLS segment refs 0 missing.
- [x] Static: all 16 JS files pass `node --check`.
- [x] Static: 0 tracking scripts, 0 external `<script>`/`<link>` dependencies.
- [x] Rendered once via the OpenDesign daemon → `RECON/screenshots/clone-desktop.png` (1440×11051).
- [ ] **Could not run** `recon-site.mjs` / `interaction-probe.mjs` / `route-crawl.mjs` /
      `visual-diff.mjs` / `audit-clone.mjs` — no Chrome/Edge/Chromium is installed on this machine
      and this task must not download one.
- **Not verified (stated honestly):** live hover/scroll/cursor behaviour, mobile/tablet layouts,
  runtime console output. Confidence comes from 1:1 source parity, not from a live click-through.

## Reproduce

```bash
& $env:OD_NODE_BIN tools/mirror.mjs   # fetch + localise the whole site
& $env:OD_NODE_BIN tools/clean.mjs    # strip tracking, fix links
& $env:OD_NODE_BIN tools/patch.mjs    # stub, placeholder, stragglers
& $env:OD_NODE_BIN tools/verify.mjs   # reference integrity
```
