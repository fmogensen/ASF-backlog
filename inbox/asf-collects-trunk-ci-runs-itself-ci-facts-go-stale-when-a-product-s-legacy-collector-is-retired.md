# ASF collects trunk CI runs itself — CI facts go stale when a product's legacy collector is retired

A product's CI facts go stale once its pre-ASF tooling is retired. The first customer's record `metrics/ci` log ends on 09-21, when its legacy collector stopped. ASF's new groom policy decide_or_close_ci_red reads those facts, so it answers on stale data or not at all: 5 CI-red Bugs got no answer because they were filed after the last recorded run.

Want: ASF collects trunk CI runs itself.
- The health or prs step appends the product's trunk CI runs (job, step, conclusion, sha, time) to the record's CI metrics, incrementally and cached, through the code-host connector or `gh run list` / `gh api`.
- doctor reports the age of the newest CI fact, and goes RED when it's older than N× the CI cadence.
- The groom policy refuses to close on facts older than its window.

Tests: an incremental append; a stale-age doctor row; the policy skips on stale facts.
