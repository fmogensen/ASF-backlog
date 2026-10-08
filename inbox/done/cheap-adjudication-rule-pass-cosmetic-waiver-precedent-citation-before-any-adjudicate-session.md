→ F-0300

# Cheap adjudication: rule pass (cosmetic waiver, precedent citation) before any adjudicate session

Parent: E-0001
priority: need

Cheap adjudication. 93 adjudicate sessions in 7 days, about 1 per 2 Features, each a full model session. Most rule on a cosmetic review demand (squash commits, wording) or on a question an existing decision or code pattern already answers.

## Acceptance
- Before an adjudicate session launches, a rule pass decides the review/coder disagreement in code where it can:
  - a C-item the product's rubric marks cosmetic (`review_rubric` severity `cosmetic`) is waived with a ruling line;
  - a C-item whose subject matches an existing decision (`decisions/`, or a docs/decisions entry) is ruled by citing it;
  - only the rest launches a session.
  The ruling is written to the same place a session ruling goes. Tested.
- The shadow decision ledger records rule-vs-session agreement, as for the other G4 deciders, so the rule set can be widened by measurement. Tested.
- Metric: adjudicate sessions per Feature, shown in the scorecard.
