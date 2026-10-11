# Bug: an answer never clears a container's Stuck; a local session limit makes the item Stuck

type: bug
parent: F-0346

Two live kernel defects (2026-10-11).

1. An operator answer never clears a container's Stuck. F-0334 (a Feature with Stories, on its plan PR) is judged Stuck(operator) "review: changes requested after 3 fix rounds" every tick, but a container with children is never stored Stuck, so facts.answer_counts dropped every answer (it required a stored Stuck before even looking at the answer's job). Two console answers keyed to review-f-0334-1791709932 (09:35Z, 09:47Z) were never applied: no ApplyAnswer, re-marked Stuck each tick. Fix: an answer naming the item's last job and not on the card counts whatever the stored state; at the fix-round cap (stored or as the judge would find it) it grants the extra round and resets the attempts to the answer marker, so the next round (a plan/spec fix session for a doc item) carries the answer and the review findings. The newest pending answer is applied, older pending ones are consumed with it; an answer is not applied while the item's session is still live.

2. A local session-limit refusal makes the item Stuck(loop). T-129173 and T-129204 ended with "You've hit your session limit · resets 2:40pm" twice and became Stuck(loop) "the session API failed". That is the account's quota, not an item failure. Fix: the local launch path classes such an end as quota for its account (stopped until the parsed reset, as #1449 does for cloud creates), records no attempt and refunds the fix round, re-fires the item the same tick on another unstopped account; with none free it waits as `seat` (NoSeat; capacity excludes stopped accounts).
