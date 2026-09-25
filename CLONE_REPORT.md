# Osmo (osmo.supply) · Original vs. Clone Report

## Conclusion

- **Complexity:** L4 — animation-heavy brand site (GSAP + ScrollTrigger + Lenis + Barba, HLS media).
  No WebGL/Canvas renderer, so below L5.
- **Mode:** Faithful clone, by true-source mirror. osmo.supply is a Webflow static export, so the
  real HTML, CSS, JS, fonts and media were downloaded 1:1 instead of being reconstructed.
- **Overall fidelity:** very high on structure, visuals and motion code; unverified on live
  interaction and responsive because no browser was available in this environment.
- **Best use:** local study of a top-tier GSAP production site, and as a high-fidelity starting
  point for a content re-skin. **Not deployable as-is** — the content is Osmo's proprietary work.

## Comparison

| Dimension | Original | Clone | Verdict |
|---|---|---|---|
| Information architecture | Webflow homepage: hero, platform, "growing toolkit", made-with-Osmo, demo vault, footer | identical DOM, untouched order | match |
| Visual language | `#f4f4f4` ground, `#201d1d` text, purple `#6840ff` + electric `#a1ff62` accents, Haffer VF, 14px/20px | original CSS + real self-hosted fonts | match |
| Motion / interaction | GSAP 3.15 suite, Lenis smooth scroll, Barba transitions, custom cursor, hover previews | byte-identical libraries, all local, code untrimmed | code match; not click-tested |
| Responsive | Webflow breakpoints (desktop/tablet/mobile) | breakpoint CSS preserved | unverified at mobile widths |
| Media | bunny.net mp4 hover videos, HLS showcase reels, Webflow CDN images | 434 images + 376 media local; HLS local at 480p | one quality trade-off |
| Content replacement | — | none (intentional) | faithful by design |
| Functional boundary | accounts, billing, checkout, gated Vault, CMS inner pages, form POSTs | stubbed / out of scope | documented |

## Scores (0-5)

| Dimension | Score | Basis |
|---|---|---|
| Source evidence | 5 | Real Webflow source mirrored to disk; every statement traceable to a file. |
| Structure | 5 | The original `index.html` DOM is kept verbatim. |
| Visual | 5 | Original CSS, real fonts, real colour tokens, real imagery. |
| Motion / interaction | 4 | All libraries + code paths local; live behaviour not exercised (no browser). |
| Responsive | 3 | Breakpoint CSS intact; only 1440px rendered. |
| Functional completeness | 4 | Nav/media/local run work; offline form POST + gated download cannot. |
| Content replacement | 1 | None — this is Osmo's own content. |
| Legal / deployment risk | 2 | Documented and clear, but genuinely high (proprietary, no licence). |

## Known gaps

1. **No live browser verification.** `recon-site`, `interaction-probe`, `route-crawl`, `visual-diff`
   and `audit-clone` could not run — no Chrome/Edge/Chromium on this machine and downloading one is
   disallowed. Verification is static (reference integrity + `node --check`) plus one daemon render.
2. **HLS showcase reels are 480p.** The 8 bunny.net HLS streams were localised at the lowest
   bitrate variant to avoid pulling the 1080p ladders (hundreds of MB).
3. **Inner pages not mirrored.** `/plans`, `/showcase`, `/vault`, `/product/*`, `/button-pack/*`
   remain absolute links to the live site. Mirroring them means re-running the harvester per route.
4. **Accounts / billing are stubbed.** Outseta is replaced by a no-op stub, so the clone is always
   in a logged-out state; the gated Vault and checkout do not function.
5. **Form submissions cannot work.** Webflow POSTs to `webflow.com/api/v1/form/`.
6. **Gated template downloads left external.** The "Lifetime Template" `.zip` links point at
   `osmo-secure.b-cdn.net` (11.8 MB + 18.5 MB). These are paid Osmo product files, so they were
   deliberately **not** copied into the clone; replace these links before any deploy.

## Suggested next steps

- Mirror the remaining routes (`/plans`, `/showcase`, `/button-pack/*`) with the same pipeline.
- Re-run `asset-harvest`/`mirror` at higher HLS quality if the project size budget allows.
- If a browser becomes available, run `interaction-probe.mjs` + `visual-diff.mjs` and `audit-clone.mjs
  --strict` for a machine-checked fidelity gate.
- To make it your own: swap copy, imagery, the `:root` palette, fonts and brand marks
  (see the replacement map in `NOTES.md`).
