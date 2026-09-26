# empty_ends park ignores the unpark stamp (re-parks an unparked item)

Follow-up to 904ffbf (unpark resets the same-head loop guard): the empty-end park (`empty_ends` in asf/workers/lifecycle.py) still counts empty ends from before the item's latest `unparked` stamp, so an unparked item can be re-parked on the next tick by the same pre-unpark evidence. Fix: count only runs started after the latest unpark (same cut-off as same_head_loop). Test: unpark after N empty ends → no immediate re-park.

## Question
Which Epic is this under? No open Epic shares a title word with it.
