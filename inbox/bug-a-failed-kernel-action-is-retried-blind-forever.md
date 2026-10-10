# Bug: a failed kernel action is retried blind forever

Bug (defect): a failed kernel action is retried blind forever (OpenPR "No commits between main and ...")

Type: bug. A fix exists on an open PR (fix branch for T-129212, which is a framework fix, not T-129212's delivery); this card owns that fix so the kernel never mistakes it for T-129212's own PR.

## Defect

Every tick logged `FAILED open PR worker/<item> ... GraphQL: No commits between main and worker/<item>`: the kernel retried the same impossible OpenPR forever, with no attempt recorded and no Stuck verdict.

## Fix

- apply: a failed action on an item (not a Launch or ApplyAnswer, which judge their own failures) is recorded as an attempt `failed: <Action> [<branch>]: <error>` (`decide.failed_attempt`).
- decide (`failed_again`, before the item's OpenPR): a failed OpenPR whose branch has no commits beyond main sends a Task/Bug back to Ready with a relaunch finding; its launch runs the still-needed gate first (Acceptance tests green on main -> Done), else rebuilds with the finding. Any other failure repeated on two consecutive ticks is Stuck(owner=loop), never a third try. An early plan's PR open is not retried a third time either.

## Acceptance (tests)

- tests/kernel/test_failed_actions.py: the loop over three ticks for the no-commits path (back to Ready with the finding, still-needed gate first) and for the repeated-failure path (Stuck(loop) on the second consecutive failure, no third try), plus the decide-level cases.

## Question
Which Epic is this under? No open Epic shares a title word with it.
