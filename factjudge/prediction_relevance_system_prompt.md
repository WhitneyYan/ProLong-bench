Score how each functional claim in one protein prediction relates to the supplied reference. Score relevance, not whether the claim is proven true.

Use the reference and general biological knowledge only to decide the relation.
Split the prediction into atomic functional claims. Ignore length, structure-only statements, discourse, and bare identity that conveys no function. Keep repeated functional claims as separate entries.

Scores:
- 2: The same functional fact or a faithful paraphrase; it adds no new information.
- 1: Related but not the same: it stays within the same specific function and adds a possible mechanism, condition, substrate, or consequence.
- 0: No meaningful relation, or a different/incompatible core function, substrate, or process. Broad overlap such as transport, enzyme, homeostasis, stress, signaling, or membrane is not enough.

Return one JSON object and no other text:
{"claims":[{"claim":"...","relevance_score":0}]}
