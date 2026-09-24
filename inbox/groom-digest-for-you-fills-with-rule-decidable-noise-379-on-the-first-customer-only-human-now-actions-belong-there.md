# Groom digest 'For you' fills with rule-decidable noise (379 on the first customer) — only human-now actions belong there

The groom digest's "For you" section must hold only genuine operator decisions: actions the approvals matrix holds at human-now. That's typically 0–5 a day, as one code-generated table. On the first customer install it read "379 for you":

1. About 250 "near-duplicate of T-…" lines. The duplicate policy compares template-shaped titles of migrated Tasks against each other. A near-duplicate is decided by rule (close the younger one when the footprint and parent match, otherwise drop the flag, and raise the title-similarity threshold for templated titles), never asked.
2. About 50 "undecided; approvals.<class> is not auto" lines, where the barred class is inferred from keywords in the card text: a parity Story read as security, legal or money. Deciding a card crosses no approval class. The class belongs to the actions the item's work will take, which are judged when the session runs (the approvals hook already does that). So decide_feature/decide_bug decide the card, and the hook guards the work.
3. "No Stories" and inbox items are groom housekeeping, not operator decisions. They move to their own section.
4. Minor: derived Backlinks are written onto removed cards. Removed and moved cards get no derived sections.

Tests: the For-you count on a fixture with templated duplicates and keyword-laden Stories is 0; a real human-now action (e.g. a new Epic) still appears.

## Question
Which Epic is this under? No open Epic shares a title word with it.
