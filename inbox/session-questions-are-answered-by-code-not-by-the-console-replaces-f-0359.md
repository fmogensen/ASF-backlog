# Session questions are answered by code, not by the console (replaces F-0359)
parent: E-0007

Type: Feature
Priority: need

## Description
Questions are answered by code, not by the console. On 2026-10-12/13, 111 of 127 session questions reached the console (13% resolved by code; the gate target is 80%). Most were a few recurring kinds with a deterministic answer in git, CI or the record. Builds on F-0339, which resolved the first two kinds.

Kinds seen most, each to be resolved in code:
- "Is this work pushed / on origin?" — read the branch head and the report commit on origin.
- "Is this red finding still true?" — a finding carries the head it was measured on; re-read the checks on the current head.
- "May I write this file?" — a test file that only proves the Task's own writes is granted by rule; anything else goes to the console.
- "I could not read the card's acceptance / the record." — the brief carries it; no question is needed.
- "A Gate command runs in no CI job." — the kernel runs read-only Gate commands on the head itself and records the result.
- "Is the predecessor still open / who owns this?" — read from the record.

## Acceptance
- [ ] Each kind above has a resolver with a test per kind, and a replay of the 2026-10-12/13 question ledger resolves at least 80% in code.
- [ ] A question the code cannot classify still reaches the console, with its kind recorded, so the next most frequent kind is visible by count.
- [ ] `asf kernel gate` row "console share" is green over a 24-hour window.

## Inputs
- Replaces F-0359, which the kernel derived as Done when a docs-only pull request (#1695, the evidence pack) merged from a branch carrying its id. Nothing of the Feature was built.
- A first spec draft exists on origin branch `spec/F-0359`; reuse what holds.
- Measured input: `docs/specs/inputs/0.4-evidence.md` on main, summary table and section A (kinds, counts, examples, queries). The raw question ledger stays on the factory host; specify the replay fixture format so a host-side step can export into it.
