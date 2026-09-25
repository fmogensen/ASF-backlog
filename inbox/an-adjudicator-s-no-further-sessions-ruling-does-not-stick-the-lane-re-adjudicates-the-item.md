# An adjudicator's 'no further sessions' ruling does not stick: the lane re-adjudicates the item

type: bug
severity: S2

An adjudicator's "no further sessions on this item" ruling does not stick. On botseon B-1372 (2026-09-25) the adjudicator ruled that the work was done and only person-merges of two PRs remained (one touches the amendable set). The next review round then went STALEMATE → ADJUDICATE B-1372 again, relaunching a paid session to re-decide a settled item, with the same loop risk on every item whose last step is a merge the factory may not make. Want: an adjudicator ruling "no further sessions" (or "waits on merge of #N") puts the item in a lane state such as WAITING_MERGE that the wave never relaunches. The lane leaves it only when the named PR merges or closes, or the head moves with new commits. `asf next` shows "WAITS ON merge: #773, #775". Tests: after that ruling, a new review round on the same head does not produce STALEMATE → ADJUDICATE; a merge of the PR moves the item on.
