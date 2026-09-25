# Osmo (osmo.supply) · Pre-deploy Audit

`audit-clone.mjs --strict` could not be executed (it needs a browser; none is installed and this
task must not download one). This audit was produced from static scans of the delivered files
instead. Re-run the script once a browser is available.

## 1. Tracking scripts — clean

Scanned `index.html` and all 16 files in `assets/js/`.

| Signature | Occurrences |
|---|---|
| `plausible` | 0 |
| `outseta.min.js` | 0 |
| `googletagmanager` / `gtag(` | 0 |
| `google-analytics` | 0 |
| `dataLayer` | 0 |
| `fbq(` / `hotjar` | 0 |

Removed: Plausible (`script.outbound-links.pageview-props.revenue.tagged-events.js` + inline queue
stub) and the Outseta SDK (`cdn.outseta.com/outseta.min.js`, the `o_options` config block, and the
`Outseta.on('signup')` revenue handler). A no-op local stub (`assets/js/outseta-stub.js`) stands in
for Outseta because site code touches `window.__outseta.profile` unguarded.

## 2. External dependencies — clean

- External `<script>` / `<link>` dependencies in `index.html`: **0**.
- `rel="preconnect"` / `rel="canonical"` links: removed.
- `integrity` / `crossorigin` attributes on local resources: removed.
- The dynamic ESM import `https://slater.app/16596/56625.js` → localised to `./assets/js/button-pack.js`.

## 3. Asset localisation — clean

- 732/732 project-relative references in `index.html` resolve to non-empty local files.
- CSS `url()` references: 0 missing. HLS segment references: 0 missing.
- All 16 JS files pass `node --check`.
- No remote image / font / media hotlinks remain (`osmo.b-cdn.net` count in `index.html`: 0).

## 4. Link risks (informational)

All remaining absolute URLs are genuine outbound navigation, not dependencies:

- **Osmo inner pages** (`osmo.supply/plans`, `/showcase`, `/product/*`) — not mirrored. Mirror them
  per-route if a multi-page clone is wanted, or repoint at local anchors.
- **Showcase credits** (a24.raviklaassens.com, dkton.at, filmbot.com, treams.com, lxlcreative.co.uk,
  paulkalkbrenner.net, somefolk.co, jordangilroy.com, eduardbodak.com, bunqlabs.com,
  dennissnellenberg.com, iljavaneck.com) — real client sites, correct to leave external.
- **Social** (instagram, linkedin, x, youtube, awwwards, slack invite) — correct to leave external.

## 5. Brand / language residue

Expected in faithful-clone mode; must be replaced before any public deploy:

- `Osmo` ×46, `osmo.supply` ×20, `osmosupply` ×7, founder names `Dennis` ×9 / `Ilja` ×9.
- `TODO` / `FIXME` / `PLACEHOLDER` markers: **0**.
- Japanese-language residue: **0**.

## 6. Licensing — HIGH RISK, unresolved by design

- The site declares **no licence**; default is all rights reserved.
- Copy, layout, illustration, video, and the **Haffer** / **Brisa Pro** fonts are Osmo's (the fonts
  are licensed commercial typefaces).
- The `osmo-secure.b-cdn.net/Lifetime%20Template/*.zip` download links in `assets/js/2b39f7-55554.js`
  point at Osmo's **paid product files** (11.8 MB + 18.5 MB). These were deliberately **not**
  copied into the project — they must be removed or repointed before deploy.
- **Verdict:** local study / remix only. Public deployment requires replacing brand, copy, fonts,
  imagery, video and the template links.

## 7. Deploy checklist

- [ ] Replace brand name, logo (`assets/img/*osmo-logo*`, `*osmo-icon-*`) and all copy.
- [ ] Swap fonts in `assets/fonts/` for licensed or open alternatives; update `@font-face`.
- [ ] Recolour via the `:root` tokens in `assets/css/4dd1ac-osmo-v2.shared.b1655ef4a.min.css`.
- [ ] Remove the `osmo-secure.b-cdn.net` template download links.
- [ ] Repoint or remove the `osmo.supply` inner-page links.
- [ ] Re-run `audit-clone.mjs --strict` with a browser for a machine-checked gate.
