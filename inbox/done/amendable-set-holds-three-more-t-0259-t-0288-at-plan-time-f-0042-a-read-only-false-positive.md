→ closed (groom 2026-09-28, adjudicator, groom-2026-09-28)

# Amendable-set holds, three more: T-0259, T-0288 at plan time, F-0042 a read-only false positive

Fresh evidence for `inbox/route-a-task-whose-writes-touch-the-amendable-set-to-the-console-at-plan-time-not-a-worker-launch.md`
(2026-09-27): three more `touch_amendable_set` holds in one afternoon, each a spent session.

- T-0259 (F-0046): `writes:` names `asf/briefs/templates/groom.md` (role_agents) from mint time.
  The coder landed the rest, left the grammar bullet out, parked; the console added it on
  `worker/T-0259` and resolved `done`.
- T-0288 (F-0055): `writes:` names `evals/manifest.json`, `evals/exemptions.json`,
  `evals/README.md` (the evals kind) from mint time. Refused twice; the test for the shipped set
  had to skip; the console added the three files on `worker/T-0288` and resolved `done`.
- F-0042 (plan): a false positive. A read-only `grep -rln "rules/R-\|asf new rule\|rule card"
  docs/specs/ docs/plans/` was refused because its command text matched `rules/*`; nothing in the
  set was written. Resolved `dropped`. So the run-time hook also matches read-only commands by
  text, which the plan-time route would not fix: the hook should judge write targets only.

Both T-cases were visible in `writes:` at plan time, exactly as the parent card predicts.

## Question
Which Epic is this under? No open Epic shares a title word with it.
