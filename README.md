# rfurze.co.uk

Source for [rfurze.co.uk](https://rfurze.co.uk) — a [Quarto](https://quarto.org)
website covering multiscale computational tribology: friction and lubrication in
artificial hip-joint bearings.

The [Methods & code](https://rfurze.co.uk/methods/) section collects runnable
notebooks demonstrating the Heterogeneous Multiscale Method (HMM), microscale
cell problems, and macro/micro coupling. Each can be opened in Colab or
downloaded.

## Building

The site is built and published to GitHub Pages automatically by
`.github/workflows/publish.yml` on every push to `main`. Notebooks are **never
executed in CI** (`execute: freeze: true`) — pages render from stored outputs,
so FEniCS/NGSolve are not installed on the build server.

To build locally:

```bash
quarto render
```

### Gotcha: `$'` in embedded notebooks

Avoid Python string literals that end with `$` and are closed with a single
quote — e.g. `ax.set_ylabel(r'$\Gamma$')`. Use double quotes instead:
`ax.set_ylabel(r"$\Gamma$")`.

Quarto's `{{< embed >}}` passes notebook source through JavaScript's
`String.replaceAll()`, where `$'` in the replacement is a special pattern
meaning "everything after the match". A single such literal makes Quarto splice
the rest of the page into the code block, repeatedly — the page renders
corrupted, and once the notebook is large enough the build dies with
`RangeError: Invalid string length`. LaTeX in matplotlib labels is the usual
source.

## Licence

This repository is dual-licensed.

- **Code** — everything in `notebooks/`, `_display/`, all code cells within
  `.ipynb` files, and the site's build configuration and styling
  (`_quarto.yml`, `styles.scss`, workflow files) is licensed under
  the **[MIT License](LICENSE-code)**.
- **Written and teaching materials** — the prose, mathematical exposition,
  figures, images and other non-code content of the `.qmd` pages and the
  markdown cells of the notebooks is licensed under
  **[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE-teaching-materials)**.

In short: reuse the code freely under MIT; reuse, adapt and teach from the
written material freely under CC BY 4.0, with attribution.

### Attribution

If you use the written or teaching material, please credit:

> Robin Furze, *Multiscale methods for lubrication* (rfurze.co.uk), 2026.
> Licensed under CC BY 4.0.

Machine-readable citation metadata is in [`CITATION.cff`](CITATION.cff).

### Third-party material

Some of the HMM material here builds on ideas developed with colleagues in the
[HMM Workshop](https://github.com/joshuamontgomery1/HMM-Workshop)
(Montgomery, Furze, Baxter-Chapman & de Boer, University of Leeds;
[doi:10.5281/zenodo.21834798](https://doi.org/10.5281/zenodo.21834798)), which is
itself released under MIT (code) and CC BY 4.0 (teaching materials). Third-party
libraries (FEniCS, NGSolve, PETSc and others) remain under their own licences.

## Contact

Robin Furze — [robin@rfurze.co.uk](mailto:robin@rfurze.co.uk) ·
[ORCID 0009-0008-6692-4982](https://orcid.org/0009-0008-6692-4982)
