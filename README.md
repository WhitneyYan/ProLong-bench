# ProLong-Bench: anonymous evaluation data

The benchmark has exactly two paper windows: **W1 and W2**. Each contains Track A and Track B. Track A is explicitly divided into retained-content A.0 and added-content A.1 evaluation sets.

## Benchmark layout

```text
benchmark/
  W1/
    Track_A/
      proteins.csv
      A.0.csv
      A.1.csv
    Track_B/
      proteins.csv
      full.csv
  W2/
    Track_A/
      proteins.csv
      A.0.csv
      A.1.csv
    Track_B/
      proteins.csv
      full.csv
```

A.0.csv and A.1.csv contain the actual reference claims, complete reference-view text and protein keys. Track_B/full.csv contains complete-target reference claims. proteins.csv in each track folder lists the full-description population; the A.0/A.1 paired evaluation uses a strict subset of Track A. Common tables retain all exact evaluation records for joining scores and predictions.

| Window | Full Track A proteins | A.0 proteins / claims | A.1 proteins / claims | Track B proteins / claims |
|---|---:|---:|---:|---:|
| W1 | 473 | 291 / 3643 | 291 / 690 | 157 / 456 |
| W2 | 372 | 243 / 4434 | 243 / 573 | 90 / 244 |

W1 has 630 full-description proteins and W2 has 462. BioReason covers 457/462 W2 full proteins and 242/243 paired proteins. Missing answers are not zero scores. benchmark/evaluation_sets.csv provides the A.0/A.1/B counts; benchmark/split_summary.csv and cohorts.csv provide cohort membership and actual model availability.

## Other contents

- `benchmark/proteins.csv`, `sequences.fasta`, `reference_claims.csv` and `reference_views.csv`: common sequences, annotations, exact reference content and claim keys.
- `predictions/`: final evaluated answers from six models, with answer hashes and cohort membership.
- `factjudge/`: exact system prompts, request parameters, user templates, scoring workflow, a real input/output example and complementary relevance-evaluation inputs.
- `scores/`: original claim/component and protein scores, full-description scores, historical-baseline scores and prediction-relevance scores.
- `baselines/`: historical-copy and homology-transfer answers with availability.
- `metrics/`: direct metric outputs and per-sample scores.
- `DATA_DICTIONARY.md`, `metadata/evaluation.json`, `CONTENTS.csv`, `MANIFEST.csv`: field definitions, scope, table sizes and checksums.

## Dates and joins

`window` is always W1 or W2; `track` is A or B. `view` identifies A.0, A.1, Track B, or whole-description reference content. `cohort` distinguishes full and strict paired population selections. Aggregate track=all combines A and B.

W1 uses baseline 2025-02-05 and target 2026-01-28. W2 uses baseline 2026-01-28 and the frozen merged target records dated 2026-06-10 or 2026-09-03. Those dates are provenance within W2, not separate windows. Full and paired source selections are independently frozen. There are 1097 unique record IDs: 1092 full-population records plus five alternate dated versions used by the paired cohort. Across them, the six model files contain 6577 evaluated answers.

Join protein records, predictions and scores with `record_id`; additionally use model and claim_id as applicable. Actual baseline_date/target_date are preserved. No internal endpoint-window labels are released. Public claim IDs include the paper window and date to preserve uniqueness; sequence-cluster aliases preserve the original grouping exactly.

The complementary relevance evaluation covers 139 original risk-screened cases across three models. It is a diagnostic subset with complete actual input texts and dates, not a full-benchmark relevance estimate. Some diagnostic inputs are outside the final benchmark source selection.

The release contains no reproduction code, model weights, API logs, credentials, human audits, matching/statistical analyses or literature checks. Citations within the original scientific text remain intact.
