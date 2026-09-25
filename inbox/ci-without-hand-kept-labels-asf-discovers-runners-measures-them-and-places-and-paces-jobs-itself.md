# CI without hand-kept labels: ASF discovers runners, measures them, and places and paces jobs itself

type: epic

Operator, 2026-09-25: "I would imagine a future where asf doesn't rely on labels … it just checks
what ci's are available and automatic adjusts queue and delegation based on registered capacity and
performance".

GitHub Actions routes a job to a self-hosted runner only through `runs-on` labels or runner groups,
so "no labels" means no human ever writes one: routing labels and groups become output ASF computes
from measured facts, never configuration. Success is the value loop's measure: queue wait and CI
minutes per Feature fall, and no gate goes red because of which box a job landed on.

Stages, each a Feature that ships on its own:
1. Discover: ASF enumerates registered runners (host API) and each box's shape (cores, memory, disk,
   runners per box) itself; ci.pool shrinks to credentials and how to reach a box.
2. Measure: every CI job's duration per runner per job kind is recorded from run logs (timing-guard
   readings are one series); each runner gets a performance score per job kind; a slow or flaky box
   surfaces by itself.
3. Place: ASF decides placement per job kind (fast boxes for timing-sensitive and critical-path jobs,
   the rest for suites) and applies it as generated labels / runner groups, or as a computed runs-on
   dispatch input; it re-places as capacity and performance move.
4. Pace: the CI cap (capacity.ci), batch sizes and launch pacing follow measured throughput and queue
   depth instead of constants.
5. Guards: timing guards take their baseline from the runner's measured class, derived from recent
   green readings.

Builds on asf/ci_pool.py (runner class, reconcile), asf/scorecard, asf/capacity.py, the cloud lane.

Acceptance:
- A newly registered runner is used with no config edit within one tick.
- A runner that turns slow is moved off timing-sensitive jobs by ASF, with one log line saying why.
- No human-written routing label remains in any product's config.
- The scorecard shows queue wait and CI minutes per Feature falling after stage 3.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.

Evidence, 2026-09-25 23:25 (a product): all 12 heavy runners busy, 5 of 7 light idle; two runs
filled every heavy box, and a trunk CI run sat queued 2h20m behind earlier PR runs (host FIFO).
- Stage 2 adds per-job resource peaks (CPU, RAM) from a runner-side sampler (post-job hook
  writing to the job summary or an artifact), beside duration and runner from the jobs API.
- Stage 3's first question: which "heavy" jobs need a heavy box (compose stacks, e2e browsers,
  timing guards) and which run on light (lint, typecheck, unit suites, docs checks) — decided on
  measured peaks; interim heuristic: a job whose light-box duration was within 1.2x of heavy.
- Stage 4 includes trunk starvation relief (built 2026-09-25): queued lower-priority runs are
  cancelled and re-run after the trunk starts.
