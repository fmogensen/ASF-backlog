# ASF collects trunk CI runs itself — CI facts go stale when a product's legacy collector is retired

A product's CI facts go stale once its pre-ASF tooling is retired. The first customer's record `metrics/ci` log ends on 09-21, when its legacy collector stopped. ASF's new groom policy decide_or_close_ci_red reads those facts, so it answers on stale data or not at all: 5 CI-red Bugs got no answer because they were filed after the last recorded run.

Want: ASF collects trunk CI runs itself.
- The health or prs step appends the product's trunk CI runs (job, step, conclusion, sha, time) to the record's CI metrics, incrementally and cached, through the code-host connector or `gh run list` / `gh api`.
- doctor reports the age of the newest CI fact, and goes RED when it's older than N× the CI cadence.
- The groom policy refuses to close on facts older than its window.

Tests: an incremental append; a stale-age doctor row; the policy skips on stale facts.

## Also: trunk CI red files an S1 Bug (first customer, 2026-09-24)

The customer's trunk went red on its CI `gate` job, and a whole-tree lint in its pre-push hook then refused every coder's non-docs push. Every Task stalled, and nothing in ASF noticed: `file-bugs` filed nothing. Once ASF collects trunk CI runs:
- A required job red on the trunk's newest run → file-bugs files or bumps one Bug `CI red: <job> on <main>` with severity S1 and decided (it's a fact, not a judgement). Its Acceptance is the job green on the trunk. The wave's S1-first order then launches its fix-bug session ahead of any Task.
- Green again on the trunk → decide_or_close_ci_red closes it.
- While it's open, harvest and the wave say "trunk red: <job>" on held code rows instead of looping corrections.
