# Repair load ≤ 3 per Feature for the release gate: count rework, not the planned first review, and cut rework
parent: E-0001

parent: E-0001

`asf release-readiness --product asf` criterion 2 (repair load) fails: 116 repair sessions / 17 Features landed in 7 d = 6.8, against `release.max_repair_per_feature: 3`. Nothing in the record tracks bringing this number under 3. F-0224 ("review takes 17 % of session spend") is about spend, not this ratio.

What the number is made of: `asf/scorecard/score.py` REPAIR_PREFIXES counts every `review` session as repair. In the asf session ledger over the last 7 d there were about 107 `review` sessions against 16 `correct` and 3 `adjudicate`. Most of the "repair load" is the one planned review each Task gets, not rework.

## Acceptance
- [ ] Decide, and record as a decision, whether a Task's first review round is repair. If it is not, `is_repair` counts only `review` rounds ≥ 2, plus correct, adjudicate, rebase, remerge, relaunch, bounce, revise and hotfix. Tests pin both cases, and `asf release-readiness` and the scorecard agree.
- [ ] The real rework drops. The mechanical pre-review (`asf review-checks`, T-0266) and `docs_review: skip` are on for the asf product wherever they apply, and the release window shows ≤ 3 repair sessions per landed Feature by the corrected count.
- [ ] `asf release-readiness` shows the repair breakdown by kind in its evidence cell, so the next reader sees which kind drives the ratio.
