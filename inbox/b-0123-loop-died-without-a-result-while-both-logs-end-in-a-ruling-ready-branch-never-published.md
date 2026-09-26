# B-0123 loop: 'died without a result' while both logs end in a ruling; ready branch never published

Proven 2026-09-26 12:51: asf held fix/B-0123 with "died twice: the session and its cold retry both ended without a result". Both job logs, ~/.ASF/logs/jobs/asf/adjudicate-b-0123.jsonl and adjudicate-b-0123-correction.jsonl, end in a `"type":"result"` record carrying a full ruling (blocked_on: none, writes: …).

Defects:
1. The dead/no-result judgement ignores a result record that is present. The hold text is false, and the item is parked for a person.
2. Both rulings say the branch is ready. The second says "only the sandbox's missing push credential stands between this HEAD and origin/fix/B-0123", and the first says "the factory now publishes this already-rebased history". The session can't push, and the factory publish didn't happen. B-0123 has been adjudicated at least 6 times today, all going around this loop.

Expected:
- A session whose log ends in a result record is never "without a result".
- When a ruling says the branch is ready and blocked_on is none, the factory publishes the worktree head (the rebased history) itself. Or the adjudicate role gets push rights for its own item's branch.
- A publish refusal is shown with the exact git error.

Tests: a result record plus an unpushed ready branch → the factory publishes it and nothing is held. A hold's text never says "without a result" when a result record exists.

## Question
Which Epic is this under? No open Epic shares a title word with it.
