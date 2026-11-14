# FactJudge-v1: scoring workflow and saved outputs

## 1. Reference construction

A.0 contains retained functional claims, A.1 later-added claims, and Track B full functional claims for newly added proteins. reference_claims.csv preserves the frozen claims and their context. The upstream decomposition prompt is gold_claim_decomposition_system_prompt.md; its request settings and exact user template are in run_configuration.json. Decomposition does not score model answers.

## 2. Reference-side support

For each protein/view/model, supply reference claims as a compact JSON array of claim_id/claim objects and the complete frozen model prediction. Apply gold_support_system_prompt.md with the exact user template and parameters in run_configuration.json. The requested evaluator was deepseek-flash, with reasoning effort low, thinking enabled and JSON-object output. The scoring request did not explicitly set temperature or a token cap. Use the model prediction alone as evidence, without outside knowledge.

The output has judgments with support_score, contradiction_score and five dimension_scores. The parser checks claim ID completeness/uniqueness, integer ranges and required component fields. Explicit contradiction requires support 0; a zero entity or relation score caps support at 1; support 4 requires both core dimensions and every applicable dimension to be 2, with contradiction 0.

example_request.json is a real request reconstructed from released reference claims and model answer. example_saved_judgments.json contains the corresponding actual saved score values reconstructed into the parsed response schema; it is not an original HTTP response envelope. All original reference-side judgments are in scores/model_claim_scores.csv, with normalized component columns. Scores link to answer hashes and original claim IDs.

## 3. Aggregation

Normalize support by dividing by 4, average claims within each protein/view, then give proteins equal weight. Mark a protein/view as Miss if every support score is below 2. Ignore inapplicable component values in component denominators; retain low applicable scores. Use actual covered proteins for each model. scores/model_protein_scores.csv and metrics/model_factjudge_summary.csv record these direct outputs.

Full-description evaluation is separately retained in full_description_claim_scores.csv and full_description_protein_scores.csv. reference_policy and score_source distinguish reused frozen scores from newly decomposed full references. It does not change original A.0/A.1/Track B scores.

## 4. Complementary prediction-side relevance

Apply prediction_relevance_system_prompt.md to a complete model answer and reference. The judge splits the answer into functional claims and assigns relevance 0-2. The exact request template is in run_configuration.json. The released saved outputs cover the original 139-case risk-screened subset (threshold >= 0.80); they are not six-model full-benchmark relevance scores. Each case's complete actual input is in relevance_inputs.csv. Exact input versions remain distinguished by dates and record_id. This release includes original final scores, not pilot variants, validation labels, raw API logs or reasoning traces.
