# DCML Beethoven Piano Sonatas Corpus

This thesis uses the **Beethoven Piano Sonatas** corpus from the [DCML (Digital and Cognitive Musicology Lab) corpora collection](https://github.com/DCMLab). The corpus PDFs and PNGs are **not redistributed** in this repository — please obtain them from the original source.

> **TODO (Katherine):** fill in the exact corpus URL and version/commit hash you used. The DCML group publishes several corpora; the one used here is the Beethoven Piano Sonatas corpus. Please paste the GitHub URL you cloned from and, if possible, the commit hash, so the corpus version is fully pinned.

## Expected layout

The pipeline notebook expects the corpus to live at a single base path, configured at the top of `notebooks/01_benchmark_pipeline.ipynb` as `BASE_DIR`. The expected sub-directory structure is:

```
<BASE_DIR>/
├── harmonies/            # *.harmonies.tsv  — used for globalkey extraction
├── measures/             # *.measures.tsv   — used for timesig extraction
├── notes/                # *.notes.tsv      — used for highest-pitch extraction
├── exports_pdf/          # *.pdf            — model inputs for PDF conditions
├── exports_png/          # *.png            — model inputs for the PNG condition
└── fewshot_examples/     # (currently unused — reserved for few-shot extensions)
```

The `harmonies/`, `measures/`, and `notes/` folders come directly from the DCML corpus. The `exports_pdf/` and `exports_png/` files are rendered from the corpus' MuseScore files (`.mscx` / `.mscz`) — see the renderer notes below.

## How to set up the corpus locally

1. Clone the corpus from its GitHub repository into a working directory.
2. Render PDFs and PNGs from the corpus' MuseScore files. The renderer used in this thesis was MuseScore's command-line export (`mscore -o`). Each movement is exported as a single PDF and a single PNG.
3. Place the rendered files into `exports_pdf/` and `exports_png/` under the same base directory as the TSV folders.
4. In `notebooks/01_benchmark_pipeline.ipynb`, set `BASE_DIR` to point at this directory.

## Why the corpus is not included here

- The DCML corpora are public, but they have their own license terms — redistributing them inside a separate repository would duplicate the canonical source and obscure version provenance.
- The rendered PDFs/PNGs are large and would inflate the repo unnecessarily.
- Anyone wanting to reproduce results runs the same extraction code over their own local copy of the corpus, which keeps the ground-truth derivation transparent.

## Citation

If you use the corpus, please cite it as instructed by the DCML repository's own README.

> **TODO (Katherine):** paste the canonical DCML citation for the Beethoven Piano Sonatas corpus here once you've checked the corpus README, so this file can be self-contained.
