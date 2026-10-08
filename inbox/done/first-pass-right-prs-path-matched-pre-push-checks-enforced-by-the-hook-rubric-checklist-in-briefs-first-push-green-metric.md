→ F-0301

# First-pass-right PRs: path-matched pre-push checks enforced by the hook, rubric checklist in briefs, first-push-green metric

Parent: E-0001
priority: need

First-pass-right PRs. Repair load is 6.4 sessions per Feature over 7 days (170 correct, 93 adjudicate, 78 review; the 1.0 target is ≤ 3). Most correction rounds fix what CI or the review would have caught before the push: package tests not run, schema or migration lists stale, write lists outside `writes:`.

## Acceptance
- A product may declare `conventions.pre_push_checks_by_path: [{paths: <glob>, run: <command>}]`. Every code session runs the commands whose globs match its diff, plus `pre_push_check`, before its one push. The githooks pre-push refuses the push when any of them fails, and the refusal names the command and the failing lines. Tested.
- Every code brief carries the matched commands verbatim, and a "before you push" checklist built from the product's review rubric (`conventions.review_rubric`, a list of short checks). Tested on golden briefs.
- A metric: `asf scorecard` shows first-push-green rate and correction rounds per Task, daily and over 7 days. Tested.
- Target: repair sessions per Feature ≤ 3 on the 7-day window. Release-readiness criterion 2 reads it.
