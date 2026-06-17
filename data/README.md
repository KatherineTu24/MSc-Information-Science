# Data

## `ground_truth.json`

A JSON array of 90 entries, one per movement in the DCML Beethoven Piano Sonatas corpus. Each entry has the following schema:

```json
{
  "movement_id": "01-1",
  "pdf_file": "01-1.pdf",
  "png_file": "01-1.png",
  "time_signature": "2/2",
  "key_signature": "f",
  "highest_pitch_bar1": {
    "tpc": "0",
    "midi": 60
  }
}
```

| Field | Description |
|---|---|
| `movement_id` | `<sonata>-<movement>` identifier (e.g. `01-1` = Sonata 1, Movement 1) |
| `pdf_file` | Expected PDF filename in the corpus' `exports_pdf/` directory |
| `png_file` | Expected PNG filename in the corpus' `exports_png/` directory |
| `time_signature` | Time signature as a string (e.g. `2/2`, `3/4`, `6/8`) |
| `key_signature` | Global key in DCML notation: **lowercase = minor**, **uppercase = major**, with `#`/`b` for accidentals (e.g. `f` = F minor, `F` = F major, `Eb` = E-flat major). Can be `null` if the movement has no annotated `globalkey` in the corpus harmonies file. |
| `highest_pitch_bar1` | Highest sounding pitch in measure 1, as `{tpc, midi}` (tonal pitch class and MIDI number). Extracted but **not used** in the current SRQ1/2/3 evaluations. |

### Movements with annotated key signature

Of the 90 movements, **63** have an annotated `globalkey` in the DCML harmonies files and are therefore used for the key signature task. All 90 are used for the time signature task. (Note: an earlier draft used 90 time-signature movements before the 10-2 correction described below — sometimes you may still see 90 in older intermediate outputs.)

## `srq2_manual_coding.csv`

Manual coding of every wrong-answer case from the SRQ2 chain-of-thought results. Each row corresponds to one `(piece_id, model, question_type)` triple that the model got wrong.

### Coding scheme

Each wrong answer is coded into one of three top-level reasoning categories, plus sub-scores for the visual reading and music-theory steps:

| Category | Description |
|---|---|
| **A** | Correct visual reading, **wrong music theory** (e.g. counted accidentals correctly, but mapped them to the wrong key) |
| **B** | **Wrong visual reading**, correct music theory (e.g. miscounted accidentals, but would have mapped them correctly given the wrong count) |
| **C** | Too vague to determine — model did not show enough reasoning to localise the error |

For **key signature**, the sub-scores cover whether the model identified the correct number of accidentals (`accidental_count_correct`), the correct type of accidentals (`accidental_type_correct`), and whether it correctly mapped from accidentals to a key name (`key_mapping_correct`).

For **time signature**, the sub-scores cover the top number (`top_number_correct`), the bottom number (`bottom_number_correct`), and whether any cut-time/common-time symbol was described correctly (`symbol_described_correct`).

The `coder_notes` column contains free-text annotations from the manual coding pass.

## Manual correction applied to ground truth

One ground-truth correction was applied after data extraction:

- **Movement 10-2** — the extracted time signature was `4/4`, but inspection of the score confirmed it should be `2/2`. The correction was applied across all three layers:
  1. The in-memory `ground_truth` list (rebuilt by the extraction cell)
  2. The cached `ground_truth.json` file
  3. All three result JSONL files (`srq1_results_fixed.jsonl`, `srq2_cot_results.jsonl`, `srq3_results.jsonl`), where the `ground_truth` field and the `correct` flag were updated for every affected row.

This correction is the reason the SRQ1 results file is named `srq1_results_fixed.jsonl` rather than `srq1_results.jsonl`.
