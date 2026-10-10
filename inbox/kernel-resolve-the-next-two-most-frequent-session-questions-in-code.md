# Kernel: resolve the next two most frequent session questions in code

Measured 2026-10-10: the console answered ~15 session NEEDS OPERATOR questions by hand. Code resolvers already cover id-claim, trunk-tests, symbol, gate-script, inbox-bug and needs-writes. Take the two most frequent remaining classes from ~/.ASF/state/asf/operator-answers.jsonl and resolve them deterministically in asf/kernel/resolvers.py, with the real answers as fixtures.
Acceptance: the kernel's per-tick "questions: resolved by code N, to the console M" share rises; tests cover both new classes with today's question texts.

## Question
Which Epic is this under? No open Epic shares a title word with it.
