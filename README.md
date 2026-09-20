# GreenSQL-RUC.github.io

Source for <https://greensql-ruc.github.io/> — the public pages for the GreenSQL
PostgreSQL energy benchmark.

## Contents

| Path | What it is |
| --- | --- |
| `index.html` | **Harness Map** — how the Makefile, shell drivers and C runners fit together, what each one reads from the kernel and from PostgreSQL, and where every measured number ends up. Three inline SVG figures and two reference tables. |
| `estimator.html` | **Benchmark Runtime Estimator** — an interactive calculator. Set the shared sweep parameters and the database scales, then split the runtime axis into stacked ranges, each with its own query count and batch sizes. Queries in a range run at its midpoint; the total and its factor breakdown update live. |
| `assets/site.css` | The shared theme — colour tokens for light and dark, base typography, and the site nav. Both pages link it. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is instead of running them through Jekyll. |

There is no build step. Each page is plain HTML: its own page-specific CSS is
inline, figures are inline SVG, and the estimator's arithmetic is a single
inline script with no dependencies. The only external requests are
`assets/site.css` and the IBM Plex webfont from Google Fonts.

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
