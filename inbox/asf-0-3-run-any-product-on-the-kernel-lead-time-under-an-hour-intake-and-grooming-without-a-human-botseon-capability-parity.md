# ASF 0.3 — run any product on the kernel: lead time under an hour, intake and grooming without a human, botseon capability parity

ASF 0.3 — a factory that runs any product, not just itself, faster and with less console.

Goal: botseon (and any product) runs on the kernel from Monday 2026-10-12 with delivery at least as good as its own swarm, and the console answers almost nothing.

Measured starting points (2026-10-10, ASF on 0.2): Tasks/Bugs done 70.7/day (old floor 21.6); first-push-green 94%; silent stuck 0; red-on-main 0; but median code PR open→merged 249 min (old 46 min); ~15 console-answered questions a day; ~10–14% of session time landed nothing new; 181 parked Features, mostly old-floor topics.

Outcomes 0.3 must deliver (each measured, each proved by a test):
1. Lead time: median code PR open→merged under 60 min, p90 under 4 h (waits ledger).
2. Intake and grooming run themselves: inbox → decided card with no human step (intake-decide verdict applied by code); a continuous sweep retires or revives parked work on idle seats (Stage 4); Definition of Ready held in code.
3. Console share: questions resolved by code ≥ 80% (per-tick counter), top recurring classes coded from operator-answers.jsonl.
4. Multi-product: one kernel host runs several products with per-product config, seats and accounts; no product special-cased.
5. botseon parity of capability, generic: a green batch landing on a moved main without a full re-run; Stories-first ordering; auto-quarantine of flaky tests with a defect check; pace control; quota-aware launches; per-account cloud environments.
6. Factory GitHub identity (GitHub App) so a busy floor cannot exhaust the operator's token.
7. Risk: two-level flag extended only if measured incidents justify it (Stage 5 trigger).

Out of scope: rewriting the decide core; new product features.

Sources: docs/specs and the 0.2 release notes (v0.2.0); the grooming design (stages 1–6 with triggers); botseon's swarm capability list (scripts/swarm on botseon's swarm/no-asf); the move plan for botseon.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.
