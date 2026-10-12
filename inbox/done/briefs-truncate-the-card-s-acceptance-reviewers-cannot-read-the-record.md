→ B-188920

# Briefs truncate the card's Acceptance; reviewers cannot read the record
parent: F-0346

Type: Bug

Review and build briefs truncate the card's Description and Acceptance, and the session cannot read the record. A cloud session has no record checkout, and a local session was refused permission on the record repo. Reviewers then judge against the plan text instead of the card, and they raise "needs input" questions asking for the acceptance block.

On 2026-10-12 this happened at least five times: review-t-154343 (permission denied on the record), review-b-173962 twice, review-t-166574 and review-t-160207. It feeds review loops: T-154343 reached 5 fix rounds.

Acceptance:
- Every brief carries the card's full Acceptance block and writes: list verbatim, never truncated. If the brief cap is hit, other sections are cut first.
- Test: a card with 12 acceptance lines produces a brief containing all 12 lines byte-identical.
