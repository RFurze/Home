# Academic homepage (Quarto + GitHub Pages)

A static personal site that showcases research and shares Jupyter notebooks as
runnable demos. Built with [Quarto](https://quarto.org), published to GitHub
Pages, and served on a custom domain.

## How it's organised

```
_quarto.yml              site config, nav, theme, fonts
_variables.yml           your GitHub user/repo/branch (used to build launch badges)
styles.scss              theme (palette, typography)
index.qmd                landing page
research.qmd             research overview
methods/
  index.qmd              auto-generated listing of method pages
  generic-1d-hmm.qmd     a worked sample (prose + launch badges + embedded notebook)
  figs/                  listing thumbnails
notebooks/               raw, runnable .ipynb files (Colab/Binder/download target)
_display/                cleaned copies used for the inline embed (auto-ignored by Quarto)
binder/environment.yml   reproducible env for Binder launches
CNAME                    your custom domain
.github/workflows/       CI that renders + publishes on every push to main
```

## The two notebook tracks

**Track A — FEniCS notebooks (e.g. the included sample).** These can't run in a
browser, so the page renders *statically* (figures are baked in) and offers
**Open in Colab** / **Launch in Binder** / **Download** buttons. Nothing is
executed when the site builds.

**Track B — pure numpy/scipy notebooks.** These *can* run live in the browser.
To make a page's code cells runnable, add the
[quarto-live](https://r-wasm.github.io/quarto-live/) extension
(`quarto add r-wasm/quarto-live`) and author the page with `format: live-html`
and `{pyodide}` cells. Not wired up by default so the build stays dependency-free.

## First-time setup

1. Create a repo and push this folder to the `main` branch.
2. Edit `_variables.yml` with your GitHub `user`, `repo`, and `branch` — this is
   what the Colab/Binder badges point at.
3. Replace the `Rob [Surname]`, `YOUR-GH-USERNAME`, and `you@example.com`
   placeholders, and put your real domain in `CNAME` and in `site-url`
   (`_quarto.yml`).
4. Push. The Action renders and publishes to a `gh-pages` branch.
5. In **Settings → Pages**, set the source to the `gh-pages` branch and enter
   your custom domain. (The shipped `CNAME` keeps the domain bound across
   deploys.) Your domain already points at GitHub Pages, so this should slot in.

## Add a new notebook

1. Drop the runnable `.ipynb` in `notebooks/`.
2. Create `methods/<slug>.qmd` (copy `generic-1d-hmm.qmd`): write the intro,
   update the badge paths and the `{{< embed >}}` target, set `title`,
   `description`, `date`, `categories`, and an `image:` thumbnail.
3. Add `"<slug>.qmd"` to the `listing.contents` in `methods/index.qmd`.

For a FEniCS notebook, regenerate its cleaned `_display/` copy (the inline embed
trims the install cell and console logs); for a pure-numpy one you can embed
`notebooks/<file>.ipynb` directly or switch it to live cells per Track B.

## Why CI never runs FEniCS

`execute: freeze: true` in `_quarto.yml` tells Quarto to render from stored
results, and the FEniCS page only embeds a pre-executed notebook. CI installs
Quarto + Jupyter purely to *read* the notebooks — FEniCS is never installed or
run on the build server.

## Local preview

```bash
quarto preview        # live-reloading local server
quarto render         # one-off build into _site/
```
