# Evaluating Multimodal LLMs for Classical Sheet Music Interpretation

Master's thesis code, data, and analyses by **Katherine (ZhaoYu) Tu**, MSc Information Studies, University of Amsterdam, 2026.

This repository contains the full evaluation pipeline for benchmarking three multimodal large language models — `claude-sonnet-4-6`, `gemini-2.5-flash`, and `gpt-5.4` — on classical piano sheet music notation tasks, using movements from the [DCML Beethoven Piano Sonatas corpus](#data).

## Research questions

The benchmark is structured around three sub-research questions (SRQs):

- **SRQ1** — How accurately do MLLMs identify key and time signatures from piano scores in a zero-shot setting? (PDF input)
- **SRQ2** — Do chain-of-thought (CoT) and role-priming prompting strategies improve accuracy, and what does the reasoning reveal about how each model fails? (PDF input, CoT prompt, zero-shot+role-priming prompt, manual A/B/C reasoning coding)
- **SRQ3** — Does the input format (PDF vs PNG) affect accuracy? (zero-shot+role-priming prompt across both formats)

Two tasks are evaluated:
- **Key signature identification** (movements with annotated `globalkey` in the corpus)
- **Time signature identification** (all movements)

## Repository structure

```
.
├── notebooks/
│   ├── 01_benchmark_pipeline.ipynb       # full pipeline: GT, prompts, API calls, runners, metrics
│   ├── 02_post_hoc_analysis.ipynb        # SRQ1 + SRQ3 analysis (accuracy, kappa, confusion matrices)
│   ├── 03_post_hoc_deep_analysis.ipynb   # music-theory-aware error analysis
│   ├── 04_srq2_coding_analysis.ipynb     # SRQ2 manual coding analysis (A/B/C categories)
│   └── archive/                          # earlier notebook versions kept for provenance
│
├── data/
│   ├── ground_truth.json                 # 90 movements, key/time signatures, highest pitch bar 1
│   ├── srq2_manual_coding.csv            # manual coding of SRQ2 wrong-answer reasoning
│   ├── README.md                         # ground truth schema + 10-2 correction note
│   └── corpus_info.md                    # how to obtain the DCML Beethoven corpus
│
├── results/
│   ├── srq1_results_fixed.jsonl          # SRQ1 raw API responses + parsed answers
│   ├── srq2_cot_results.jsonl            # SRQ2 raw CoT responses + parsed answers
│   └── srq3_results.jsonl                # SRQ3 raw responses (PDF + PNG conditions)
│
├── outputs/
│   ├── figures/                          # confusion matrices, plots
│   └── tables/                           # exported CSV summaries (srq2_summary.csv etc.)
│
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

## Setup

**1. Clone and install:**
```bash
git clone https://github.com/KatherineTu24/MSc-Information-Science.git
cd MSc-Information-Science
pip install -r requirements.txt
```

If you want byte-identical versions of every package (the exact environment used to produce the committed results), use the pinned file instead:

```bash
pip install -r requirements_frozen.txt
```

`requirements.txt` lists only the top-level dependencies and lets pip resolve transitive versions, which is friendlier for forward compatibility. `requirements_frozen.txt` is a `pip freeze` snapshot of the full dependency tree at the time the thesis results were produced.

**2. Configure API keys:**
```bash
cp .env.example .env
# then edit .env and add your three API keys
```

**3. Obtain the corpus.** See [`data/corpus_info.md`](data/corpus_info.md) for instructions on downloading and placing the DCML Beethoven Piano Sonatas files. The PDFs and PNGs are **not** included in this repository.

## Reproducing the results

The repository ships with the raw result JSONLs in `results/`, so you can run all analyses without re-querying the APIs. The notebooks are designed to be run in order:

1. **`01_benchmark_pipeline.ipynb`** — Builds `ground_truth.json` from the corpus TSV files and runs the SRQ1/2/3 evaluations. Skip the runner cells if you only want to use the shipped result files.
2. **`02_post_hoc_analysis.ipynb`** — SRQ1 and SRQ3 quantitative analysis (accuracy tables, confusion matrices, Cohen's Kappa, per-class accuracy, McNemar's test, cross-format consistency).
3. **`03_post_hoc_deep_analysis.ipynb`** — Music-theory-aware error analysis (relative major/minor confusion, enharmonic equivalence, accidental direction, circle-of-fifths distance, shared failure modes).
4. **`04_srq2_coding_analysis.ipynb`** — SRQ2 CoT reasoning analysis using the manually coded A/B/C categories.

> **Note:** Running the full pipeline from scratch will issue several hundred API calls across three providers and incurs cost. The results are crash-safe and resume from the last completed entry.

## Data

Ground truth is extracted from the **DCML Beethoven Piano Sonatas** corpus (see [`data/corpus_info.md`](data/corpus_info.md)). The repository contains:

- The processed `ground_truth.json` (90 movements, with one manual correction documented in [`data/README.md`](data/README.md))
- The SRQ2 manual coding sheet covering all wrong-answer cases

The corpus PDFs and PNGs themselves are not redistributed here.

## License

No license. All rights reserved by the author. Code is published for academic transparency and reproducibility of the thesis results; please contact the author for any other use.

## Contact

Katherine (ZhaoYu) Tu — MSc Information Studies, University of Amsterdam.
Contact email: katherine.tu@student.uva.nl
