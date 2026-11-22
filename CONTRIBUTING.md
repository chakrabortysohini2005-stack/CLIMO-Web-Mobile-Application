# 🤝 Contributing to CLIMO

Thanks for helping CLIMO read the sky for more people. This guide keeps changes easy to review.

## Getting set up

No toolchain is needed. Clone the repository and serve it:

```bash
git clone https://github.com/chakrabortysohini2005-stack/CLIMO-Web-Mobile-Application.git
```

```bash
cd CLIMO-Web-Mobile-Application && python3 -m http.server 8080
```

Open <http://localhost:8080/CLIMO-fixed/CLIMO-project/CLIMO_backend.html>.

## Workflow

1. **Open an issue first** for anything larger than a typo, using the bug or feature template.
2. Branch from `main`, with a name like `feat/route-eta`, `fix/drawer-timezone` or `docs/gallery`.
3. Keep each PR focused on one feature or fix.
4. Fill in the PR template and include **before/after screenshots** for UI changes.
5. Every PR to `main` runs the Android build. Make sure it passes.

## Code style

- **Vanilla JS, no build step.** Don't add bundlers or frameworks without discussing it first.
- New dashboard code goes inside the main IIFE in `js/app.js`, grouped under a `/* ===== Section ===== */` header like the existing ones.
- Use the shared helpers. Don't write your own versions of them:
  - colours and thresholds → `riskColor()`, `riskLabel()`, `RISK_THRESHOLDS`
  - distance → `haversineKm()`
  - timestamps from Open-Meteo → `parseIST()`
  - user or remote text going into `innerHTML` → `escapeHTML()`
- Batch multi-point Open-Meteo requests (comma-separated lat/lon) instead of sending one request per point.
- Wrap `localStorage` in `try/catch`, because it throws in sandboxed WebViews.
- Comments should explain **why**, as the existing ones do.

## Common contributions

### Add a station

Add an entry to `INDIA_STATIONS` in `js/app.js`:

```js
{id:43XXX, code:"IMD-ABC-43XXX", name:"City", state:"State", type:"AWS", lat:12.3456, lon:78.9012},
```

- `type` must be one of `AWS`, `rain_gauge`, `radar` or `river_gauge`.
- To include the station in the 2000–2030 heatmap, add 31 matching rows (one per year) to `WEATHER_DATA` with the same `Station_ID`.

### Add or improve a language

1. Add `{code, native, english}` to `LANGUAGES`.
2. Add a block to `TRANSLATIONS` with the same keys as `en`. Missing keys fall back to English.
3. In the HTML, mark text with `data-i18n="key"` and placeholders with `data-i18n-placeholder="key"`.

### Tune the risk model

Change weights or caps only in `computeRisk()`, and update [docs/RISK_MODEL.md](docs/RISK_MODEL.md) in the same PR.

### Update gallery screenshots

Name the files `NN-short-name.webp`, at about 1920 px wide for desktop and 780 px for mobile, quality around 82. Put them in `docs/gallery/`, then add them to [docs/GALLERY.md](docs/GALLERY.md) and to the `SHOTS` array in [docs/gallery/index.html](docs/gallery/index.html).

## Commit messages

Use the imperative mood and an optional scope:

```text
feat(route): show ETA delta in minutes
fix(timeline): pin Open-Meteo timestamps to IST
docs: add Odia translation notes
```

## Reporting security issues

Please **don't** open a public issue. See [SECURITY.md](SECURITY.md).
