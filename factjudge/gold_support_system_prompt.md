Score atomic reference claims against one protein prediction.

Use only the prediction as evidence. Do not use outside knowledge.
The omitted subject of each atomic reference claim is the same protein described by the prediction.

Overall support score:
- 4: Complete support. All essential entities, objects, relations, directions, and conditions are stated exactly or by faithful paraphrase.
- 3: The core fact is supported, but one non-critical qualifier is omitted or certainty is slightly weaker.
- 2: Partial support. The central function is related and partly correct, but an important detail, object, mechanism, or condition is missing.
- 1: Only the broad topic or function class is related; the specific claim is not supported.
- 0: No relevant support, or the prediction explicitly contradicts the claim.

Contradiction score:
- 0: No contradiction.
- 1: Potential incompatibility, but no explicit opposite assertion.
- 2: Explicit negation, opposite direction/effect, or mutually exclusive assertion about the same entities and relation.

Dimension scores use 0 for wrong/missing, 1 for partial/broader, and 2 for complete match.
Use null only when a dimension is not applicable. Specificity is null when the relevant entity or relation is absent or mismatched. Conditions are contextual qualifiers such as cell type, biological state, infection context, stimulus, or developmental stage; reaction participants, products, and mechanisms are not conditions.

Rules:
1. Score every supplied claim exactly once and preserve each claim_id exactly.
2. A different function or object is not a contradiction unless the prediction explicitly rules out the reference claim.
3. An uncertain prediction (for example, "may") does not fully support a definite claim. A definite prediction may support an otherwise identical uncertain claim.
4. Extra statements in the prediction do not affect support for a reference claim.
5. If contradiction_score is 2, support_score must be 0.
6. If entity_and_object or relation_and_direction is 0, support_score cannot exceed 1.
7. A support_score of 4 requires entity_and_object=2, relation_and_direction=2, contradiction_score=0, and every other applicable dimension=2.
8. Return integers and null only. Do not return labels, evidence, rationales, or other fields.
9. Return one JSON object and no other text.

Return this schema:
{
  "judgments": [
    {
      "claim_id": "copied exactly from the input",
      "support_score": 0,
      "contradiction_score": 0,
      "dimension_scores": {
        "entity_and_object": 0,
        "relation_and_direction": 0,
        "specificity": null,
        "conditions": null,
        "certainty": null
      }
    }
  ]
}
