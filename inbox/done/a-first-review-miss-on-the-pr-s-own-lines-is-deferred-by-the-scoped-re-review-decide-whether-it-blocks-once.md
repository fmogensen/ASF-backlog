→ B-216547

# A first-review miss on the PR's own lines is deferred by the scoped re-review; decide whether it blocks once
parent: F-0358

Type: Bug

Under kernel.review.rereview_scope: fix, a re-review finding on a line the PR itself changed but the fix did not (a first-review miss) is LATER and lands (asf/kernel/findings.py `split_scope`, PR #1735). Replayed on 12 re-reviews: 9 of 21 findings, 4 approvals, all 9 real defects on files the PR changed. Decide by measurement whether such a finding blocks once per item: read the PR's own diff (merge base to head) through RealGitHub.diff_lines, and block a new-key finding inside it on the first re-review only.

## Acceptance
- [ ] A typed reviews row counts LATER findings inside the PR's own diff apart from those outside it (tests/kernel/test_review_measure.py).
- [ ] With the once rule on, such a finding blocks on the first re-review and is LATER on the next (tests/kernel/test_rereview_scope.py).
- [ ] An unread PR diff keeps the finding blocking.
