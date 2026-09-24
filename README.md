# GreenSQL-RUC.github.io

Source for <https://greensql-ruc.github.io/> — the public pages for the GreenSQL
PostgreSQL energy benchmark.

## Contents

| Path | What it is |
| --- | --- |
| `index.html` | **Harness Map** — how the Makefile, shell drivers and C runners fit together (including the slope sample's order-file replay), what each one reads from the kernel and from PostgreSQL, and where every measured number ends up. Three inline SVG figures, the slope sample's error-by-band table, and two reference tables. |
| `estimator.html` | **Benchmark Runtime Estimator** — an interactive calculator for warm step-up sweeps. Set the shared parameters and the database scales, then split the single-copy runtime (or energy) axis into stacked ranges, each with its own query count, energy band and batch config. The total, and how it splits into restart + gate, warm-ups and measured batches, update live. |
| `assets/site.css` | The shared theme — colour tokens for light and dark, base typography, and the site nav. Both pages link it. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is instead of running them through Jekyll. |

There is no build step. Each page is plain HTML: its own page-specific CSS is
inline, figures are inline SVG, and the estimator's arithmetic is a single
inline script with no dependencies. The only external requests are
`assets/site.css` and the IBM Plex webfont from Google Fonts.

## The estimator's model

Each entry is timed from what the slope sample measured (21–23 Sep 2026,
i5-8350U at 2.5 GHz, PG 18.4):

```
entry = restart + gate + t1·(cold[band] + w − 1) + gap + Σ over measured N of (t1·r[band][N] + gap)
total = repeats × pg × indexing × distribution × Σ ranges queries × Σ scales entry(t1 = mid × scale)
```

`t1` is one warm copy at scale 1 and `w` the number of warm-ups, which run in
one process with the first one cold. The defaults are restart + ready 3.74 s,
gate 2.78 s, gap 0.077 s, and per band (by single-copy energy, picked from
`E1 ≈ P × t1` with P = 10.2 W) the cold ratio and the batch-time ratios
`r = T_N / T_1`. All of them sit in the page's *Measured overheads* panel and
in `DEFAULTS` at the top of its script. At each band's median `t1` with two
warm-ups, the model gives the published entry times to within 0.1 s for
`{1}`, `{1,16}` and `full`, except 75–120 J `full`: 286.7 s against 286.8 s,
which is inside the rounding of the two-decimal ratios.

The old linear model (`N × t1` per batch, no overheads, scales multiplying
everything) is still shown in the result rail for comparison.

## Theming

Colours live as CSS custom properties in `assets/site.css` and nowhere else —
`--bg`, `--panel`, `--ink`, `--muted`, `--line`, `--faint`, `--accent`, and the
measurement-source accents (`--energy`, `--thermal`, `--server`, `--client`).
Each is defined twice, once for light and once for dark, so the pages follow the
reader's `prefers-color-scheme`. Change a colour there and both pages move
together; adding a page means linking the same stylesheet.

## Publishing

This is a user/organisation Pages repository, so GitHub serves the root of the
default branch (`main`) at `https://greensql-ruc.github.io/`. Pushing to `main`
publishes; a deploy usually appears within a minute.

If the site is not live yet, enable it once under
**Settings → Pages → Build and deployment**, with *Source* set to
**Deploy from a branch** and the branch set to **`main` / `/ (root)`**.

## Editing

The pages load `assets/site.css` by relative path, so opening an HTML file
straight from disk leaves it unstyled. Serve the directory instead:

```bash
python3 -m http.server 8000
```

Then visit <http://localhost:8000/>.
