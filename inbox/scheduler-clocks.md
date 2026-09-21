# Scheduler clocks are configuration: each job group on its own interval, per product
type: feature

## Description
As an operator, I want to say in the product's yaml how often each group of steps runs — the
record every 5 minutes, dispatch every 10, the daily at 06:50, the shadow tick every 30 — so that
a clock is a line in a file, not a flag someone typed at install time.

Today `asf scheduler install --steps … --interval …` takes the clock from the command line and
`scheduler.interval_s` in `config.yaml` is one number for everything.

## Acceptance
- [ ] `products/<p>.yaml` gains a `schedule:` block — a list of jobs, each `{name, steps, every}`
  where `every` is a duration (`5m`, `10m`, `1h`) or a clock time (`06:50`, daily); e.g.
  `record: {steps: [record], every: 5m}`, `dispatch: {steps: [health, wave, prs, batch], every: 10m}`,
  `daily: {steps: [daily], every: "06:50"}`. `config.yaml` may carry `scheduler.defaults` with the
  same shape, used when a product declares nothing.
- [ ] `asf scheduler install --product <p>` (no other flags) renders and loads one scheduler job
  per `schedule:` entry — label `<prefix>.<product>.<name>` — and removes jobs the block no longer
  names; `--steps/--interval` stay as an override for a one-off and print that they are one.
- [ ] `asf scheduler status --product <p>` prints one row per declared job: name · steps · every ·
  state · runs · last exit · last run age; a declared job that is not loaded is a red row.
- [ ] `asf doctor` shows the same rows and flags a job whose interval is shorter than its last run's
  duration (`record every 5m but the last run took 6m 10s` — the clock is lying).
- [ ] A change to `schedule:` is applied by the next `asf scheduler install` (idempotent); the
  cutover kit calls it instead of choosing steps and intervals itself.
- [ ] Adapter coverage: launchd (`StartInterval` / `StartCalendarInterval`), cron (`*/5 * * * *`,
  `50 6 * * *`); systemd timers as a later Story.

## History
- 2026-09-21 19:35 operator: "the scheduler should have a configuration, so that ticks can run on
  different clocks, ie every 5 min …"

## Question
Which Epic is this under? No open Epic shares a title word with it.
