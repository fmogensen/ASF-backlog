# DONE table credits 'landed <sha>' to a run with no commits of its own

The DONE table credits a run with "landed <sha>" when the sha is only the trunk tip its branch was cut from. Two correction runs with no commits of their own showed "finished, landed 548fe32", where 548fe32 was another item's plan merge. The item itself correctly stays Active, so this is a display misattribution, but it misleads the operator. Reported by the first customer install.

Fix: "landed <sha>" requires a commit on the run's branch that is not an ancestor of the base it was cut from, or the run's own harvest mark. Otherwise show "finished, nothing of its own". Test: a branch with no own commits → no landed sha.

## Question
Which Epic is this under? No open Epic shares a title word with it.
