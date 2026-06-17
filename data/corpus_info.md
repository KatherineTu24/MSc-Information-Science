# DCML Beethoven Piano Sonatas Corpus

This thesis uses the **Beethoven Piano Sonatas** corpus from the [DCML (Digital and Cognitive Musicology Lab)](https://github.com/DCMLab) corpora collection. The corpus PDFs and PNGs are **not redistributed** in this repository — please obtain them from the original source.

- **Corpus repository:** https://github.com/DCMLab/beethoven_piano_sonatas
- **Commit used in this thesis:** `ea7181bff88abc8713257234f7ec4033178c57a9`
- **License:** Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International ([CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/))

## Expected layout

The pipeline notebook expects the corpus to live at a single base path, configured via the `CORPUS_DIR` environment variable (read from `.env`). The expected sub-directory structure under that base path is:

```
<CORPUS_DIR>/
├── harmonies/            # *.harmonies.tsv  — used for globalkey extraction
├── measures/             # *.measures.tsv   — used for timesig extraction
├── notes/                # *.notes.tsv      — used for highest-pitch extraction
├── exports_pdf/          # *.pdf            — model inputs for PDF conditions
├── exports_png/          # *.png            — model inputs for the PNG condition
└── fewshot_examples/     # (currently unused — reserved for future few-shot extensions)
```

The `harmonies/`, `measures/`, and `notes/` folders come directly from the DCML corpus. The `exports_pdf/` and `exports_png/` files are rendered from the corpus' MuseScore source files.

## How to set up the corpus locally

```bash
# 1. Clone the corpus at the pinned commit
git clone https://github.com/DCMLab/beethoven_piano_sonatas.git
cd beethoven_piano_sonatas
git checkout ea7181bff88abc8713257234f7ec4033178c57a9

# 2. Render PDFs and PNGs from the .mscx files using MuseScore CLI
#    (example for a single file — script the loop as needed)
mscore -o exports_pdf/01-1.pdf 01-1.mscx
mscore -o exports_png/01-1.png 01-1.mscx

# 3. Point CORPUS_DIR in your .env file at this folder
echo "CORPUS_DIR=$(pwd)" >> /path/to/MSc-Information-Science/.env
```

## Why the corpus is not redistributed here

- The corpus has its own license (CC BY-NC-SA 4.0) that requires attribution and prohibits commercial use. Keeping it at its canonical source preserves clear provenance.
- The rendered PDFs/PNGs are large and would inflate this repository.
- Anyone wanting to reproduce results runs the same extraction code over their own local copy, which keeps the ground-truth derivation transparent.

## Citation

If you use the corpus, please cite the corpus data report:

> Hentschel, J., Rammos, Y., Neuwirth, M., Moss, F. C., & Rohrmeier, M. (2024). An annotated corpus of tonal piano music from the long 19th century. *Empirical Musicology Review*, 18(1), 84–95. https://doi.org/10.18061/emr.v18i1.8903

The corpus is also part of the broader **Distant Listening Corpus** infrastructure, which has its own accompanying paper:

> Hentschel, J., Rammos, Y., Neuwirth, M., & Rohrmeier, M. (2025). A corpus and a modular infrastructure for the empirical study of (an)notated music. *Scientific Data*, 12(1), 685. https://doi.org/10.1038/s41597-025-04976-z
