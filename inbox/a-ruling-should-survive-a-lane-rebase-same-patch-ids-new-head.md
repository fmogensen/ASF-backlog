# A ruling should survive a lane rebase (same patch-ids, new head)

Follow-up to the B-1377 adjudicate loop (fixes 5103a34, bdd0fb7, 4667643): a ruling stands when its launch_head equals the head, or the head is the named sha plus review files only. It does not stand when the lane itself rebases the branch onto trunk AFTER the ruling (same patches, new shas), so one extra adjudicate launches before the loop guard parks. Fix: compare patch-ids (git patch-id over the branch's own commits) between the ruled head and the current head; equal patch sets keep the ruling. Test: a lane rebase after a done ruling launches no adjudicate.

## Question
Which Epic is this under? No open Epic shares a title word with it.
