# A correction on a gated/landing item never surfaces (row shows empty correction)

2026-09-27 01:0x botseon: corrections set on items whose lane state is a landing wait (GATE / "WAITS ON landing: PR #707 GATE" — cloud/team-staffing-t3; PR #842 cloud/spec-tinkerer-mode at GATE, review: none) are recorded (stamped, 30828f4) but never surface: `asf next` shows the row with an empty correction and no session launches, because only "back to a session" states produce a FIX → CORRECT row. Those PRs can never go green without the correction (redaction/re-book content fixes the hook and bands check need).

Fix: a stamped, unanswered correction on an item whose branch is in a landing-wait state (GATE/WAITING/PR_OPEN) moves the lane to BACK (kind = the correction's kind) and yields a FIX → CORRECT row with that text, subject to the round cap/loop guard. Test: gated item + fresh correction → FIX → CORRECT row carrying the text.

## Question
Which Epic is this under? No open Epic shares a title word with it.
