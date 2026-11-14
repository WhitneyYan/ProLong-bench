# Data dictionary

## Window, track, view and population

- `window`: exactly W1 or W2, using the final merged paper definition.
- `track`: A for existing proteins, B for newly added proteins; `all` occurs only in aggregate metrics combining both.
- `view`: A.0 is retained reference content and A.1 is added reference content within Track A. Track B uses its complete target reference. `full` in the same-cohort metrics denotes full references for the strict paired Track A cohort; `full_population` denotes the complete population across both tracks.
- `cohort`: full or paired. The full population contains 630 W1 and 462 W2 proteins. Paired contains 291 W1 and 243 W2 Track A proteins. A.0 and A.1 evaluate the same frozen answer for each paired protein.
- `baseline_date`, `target_date`: actual dates of each annotation record. Two target dates within merged W2 do not define extra windows.

## Record and claim keys

`record_id` is paper-window + target-date + accession. It uniquely distinguishes the exact evaluation text version. `accession` is a UniProt accession. `fasta_id` equals record_id; sequence_sha256 hashes the exact amino-acid sequence. `benchmark/cohorts.csv` is authoritative for membership, applicable sequence clusters and model availability.

`benchmark/proteins.csv` contains one row per record_id. Its 1097 records comprise 1092 full-population records plus five alternate versions needed by the paired population. The per-track proteins.csv files contain full-population records only; A.0/A.1 claim tables identify the exact paired records. Filter membership flags rather than counting alternate versions as extra proteins.

Predictions have one row per record_id/model; prediction_sha256 hashes the trimmed evaluated answer. Join model scores to reference_claims.csv by record_id and claim_id, and to predictions by record_id/model. Whole-description scores contain full_claim_id and the reference claim itself; original_claim_id identifies reused reference claims. Public IDs consistently encode the merged paper window and actual date; they are aliases for saved evaluation IDs. No score or reference text is changed by this relabeling.

`sequence_cluster` is a one-to-one anonymous alias of the original cluster key. Identical original clusters keep identical aliases across tables; different original clusters remain different. Cohort-specific cluster assignments are retained. `model` denotes the evaluated system; `judge_model` the evaluator. Original context/answer hashes retain their original text-normalization scope. MANIFEST.csv hashes released files.

## FactJudge scores

Support is 0-4, contradiction 0-2, and each component 0-2 or NA. Exact rules and request settings are in factjudge/. NA means not applicable, not zero. Components are entity/object, relation/direction, specificity, contextual conditions and certainty.

Protein recall is mean(support_score/4) for each protein/view. Miss is true when no reference claim receives support >= 2. Overall recall/Miss are equal-protein means. Missing model answers and unavailable homology hits are not zero scores. metrics/model_factjudge_summary.csv directly aggregates scores/model_protein_scores.csv with actual N and claim/cluster counts.

Full-description and paired-full scores remain separate populations. Full references reuse frozen scores only when exact statement-partition coverage holds; Track B uses its existing full reference; remaining full references were independently decomposed/scored. Two view-level macro means were never simply averaged. reference_policy and score_source retain provenance.

## Traditional metrics and baselines

ROUGE-L and BioBERT BERTScore are protein means. Main deterministic fact F1 is 2*sum(TP)/(sum(generated)+sum(reference)); protein-macro F1 is also reported. Its deterministic fact counts differ from FactJudge reference-claim counts.

Historical copying uses the same protein's baseline annotation. Homology transfer uses the fixed 2025-02-05 library, excludes the same accession and requires identity >= 0.30 and query/target coverage >= 0.80. Availability flags are authoritative. No statistical model-versus-baseline contrasts are included.

## Complementary prediction relevance

factjudge/relevance_inputs.csv contains the exact diagnostic case texts; join prediction_relevance_claim_scores.csv by case_id and record_id. claim_index enumerates predicted claims. Relevance 2 means the same fact/paraphrase, 1 a related extension and 0 a reference-relative mismatch; it does not independently establish biological falsity. The 139 cases were risk-selected (threshold >= 0.80), not randomly sampled from the final benchmark. Their actual input dates remain explicit and do not imply benchmark membership. No human labels or agreement analyses are included.
