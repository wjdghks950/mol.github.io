# Mixture of Layers project website

A responsive, buildless research website. `index.html`, `styles.css`, and `app.js` run on any static host, including GitHub Pages. Asset URLs are relative so the same source works at a domain root or `/mol.github.io/`.

## Preview

From the repository root:

```sh
python3 -m http.server 8765 --directory website
```

Open `http://localhost:8765`. No Node.js or build step is needed. Google Fonts is the only external presentation dependency; system font fallbacks keep the site usable offline. Scores and images are served locally. No analytics or tracking is included.

## Content and provenance

- Authors and title: `paper/draft/neurips_2024.tex`.
- Narrative: abstract, introduction, method, and experiments in `paper/draft/sections/`.
- Interactive results: the labeled **main backbone table** and the **dual-encoder rows** of the single-/multi-encoder comparison in `experiments.tex`. All 176 scores are included, including regressions. Deltas are recomputed from displayed scores, not copied from rounded/inconsistent delta rows.
- Ablations: the displayed text-encoder, reserve, and k-ablation tables in `experiments.tex`. These are separate experimental settings and are not merged into the main results.
- Figures: optimized WebP exports of the original PDFs in `paper/draft/figures/`. Hero and router diagrams are conceptual illustrations, not measured routing probabilities.
- Paper links point to the official arXiv abstract page: https://arxiv.org/abs/2610.09440. The bundled `assets/mixture-of-layers.pdf` remains a snapshot of `paper/draft/neurips_2024.pdf`.
- `source-manifest.json` records SHA-256 hashes of the paper inputs used for this snapshot. Citation metadata describes the current 2026 manuscript and has no invented DOI or arXiv identifier.

### Draft inconsistencies handled deliberately

The repeated CLIP rows in the feature-integration comparison disagree with the first backbone table on CharXiv and hybrid MMStar. The site consistently uses the first table for its backbone explorer. The text-encoder ablation prose disagrees with its numeric table; the site uses the displayed 77.31 / 84.03 scores. The reserve and k ablations have different scores from the main comparison and remain explicitly labeled as separate ablations. The multi-encoder section states that encoder and fusion differences prevent a routing-only causal claim.

Review these source tables when finalizing the paper. Refreshing extracted data does not automatically rewrite narrative or ablation text in the HTML.

## Refresh from the paper

Use a Python environment with PyMuPDF and Pillow, and point `--paper-dir` at the current paper draft in the research checkout:

```sh
python3 website/scripts/refresh_content.py --paper-dir ../MoL/paper/draft
python3 website/scripts/check_site.py --paper-dir ../MoL/paper/draft
```

This updates `results-data.js`, CSV/JSON downloads, figure images, the downloadable paper, and source hashes. The generator fails if the expected table layout changes. The dependency-free check validates local links, fragments, score consistency, CSV parity, headline claims, and the PDF. When the source paper is present, it also checks every extracted score against the LaTeX.

## GitHub Pages deployment

The workflow `.github/workflows/project-website.yml` uploads **only `website/`**, checks assets and results, and deploys through GitHub Pages. It runs automatically for website changes on the default branch, or manually with **Actions → Deploy paper website → Run workflow**. Other branches do not automatically publish. Compiled assets are checked in, so GitHub does not need the paper sources or Python imaging packages.

The deployment job has `contents: read`, `pages: write`, and `id-token: write` permissions. Pages uses **GitHub Actions** as its publishing source and the `github-pages` environment.

Repository: `wjdghks950/mol.github.io`. Site: **https://wjdghks950.github.io/mol.github.io/**. Canonical and social preview metadata use this address. The research code remains in `SStoica12/MoL`; this separate repository contains the website and its published assets.

See [GitHub’s Pages creation guide](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site) and [custom workflow guide](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages).

## Browser checks

The page supports keyboard-operated routing and encoder buttons, a native backbone select, scrollable full result tables, an Escape-dismissable figure dialog, reduced motion, and clipboard copying with a text-selection fallback. Run browser QA against a root URL and a `/mol.github.io/` subpath after changing assets or navigation. Check narrow screens, each backbone, each router, both attention figures, CSV/PDF downloads, citation copy, and Escape/focus restoration in the dialog.
