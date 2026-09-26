# CI queue status shows total wait, not the at-head age the starvation guard uses

954ee3e's starvation guard counts minutes AT the head (head_since), but the status line prints total wait ("head T-0084 waits 129 min"), so a head well under head_wait_max_min looks starved and un-admitted (botseon session asked why at 21:32). Status should print both: "head T-0084 waits 129 min (12 min at the head; admitted at 20)". Test: status text shows the at-head age and the guard threshold.

## Question
Which Epic is this under? No open Epic shares a title word with it.
