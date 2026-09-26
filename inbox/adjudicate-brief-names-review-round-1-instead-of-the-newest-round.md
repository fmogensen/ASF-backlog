# Adjudicate brief names review round 1 instead of the newest round

The adjudicate brief names the first review file, not the newest. For botseon T-0141 it reads "Review file to answer: .sdd-input/reviews/1-t-0141.md (round 1)", but the latest is 5-t-0141.md (round 5, approved).

Expected: the brief picks the review file with the highest round number.

Test: a branch with review files for rounds 1-5 → the brief names round 5.
