# Corrections of mechanical failures run on Opus: 25% of all spend goes to correct/adjudicate/groom sessions
parent: E-0002

Bug, severity S2, parent E-0002. Decided by the operator on 2026-09-23 ("fix first so we get an effective production before expansion"; cost and speed are the goal).

Session spend to date is $365. Opus sessions of kind other (corrections, adjudications and grooms) cost $91.50, which is 25% of all spend, at an average of $1.66 each (55 sessions). That is almost as much as all spec writing ($88.72) and more than twice all fix-bug work ($25). Many of them are corrections of mechanical failures ("not pushed", "unpushed work", "empty branch") that need no Opus judgement. B-0080's title notes that corrections and adjudications launch on Opus regardless.

Expected:
- The model is chosen by the job's need, not its kind. A correction of a mechanical failure (push, commit, rebase, a red gate with a named test) runs on Sonnet. Opus is kept for the adjudicate row at the round limit, specs, plans and the groom adjudicator.
- The model per job kind is operator config (models: {correct: sonnet, adjudicate: opus, …}) with these defaults.
- The daily rollup reports spend by kind × model, and the Opus share is a tracked metric with a threshold that files a Bug.
- Tests cover model selection per row.
