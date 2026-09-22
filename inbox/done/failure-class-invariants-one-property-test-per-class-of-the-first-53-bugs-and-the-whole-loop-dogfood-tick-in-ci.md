→ F-0087

# Failure-class invariants: one property test per class of the first 53 Bugs, and the whole-loop dogfood tick in CI
parent: E-0001
type: feature

Operator, 2026-09-22 10:30: "What are we doing to reduce errors like these?" — 53 Bugs in 36 hours, each
fixed with a failing-first test; the same instance cannot recur, the next kind still can. The classes:

| Class | Bugs | Invariant test |
| --- | --- | --- |
| registry semantics | B-0028, B-0041, B-0051 | every launch line opens a run; a run's terminal fields never fold into the next; a `finished` run has a pushed branch — property test over generated registries |
| environment leaking into gates and tests | B-0033, B-0038, B-0043, B-0047 | the suite and the harvest gate are hermetic: run under `env -i` + the operator's own `ASF_HOME`/`ASF_PRODUCT` + a foreign `init.defaultBranch` in CI's matrix |
| path resolution | B-0036, B-0042, B-0050 | every command runs from `/tmp` with `--product`, creates nothing under `/tmp`, and prints `record:`/`repo:` on its first line — the audit's matrix as a test |
| branch lifecycle | B-0025, B-0046, B-0048, B-0049 | a state machine: launched → pushed → held(n) → corrected → landed → reaped; every transition has a test, no state is terminal except reaped/adjudicated |
| worker behaviour | B-0024, B-0052 | every brief ends with the closing paragraph; a result with unpushed work is failed at the source |
| push races | B-0030, B-0044 | origin moves between fetch and push in every record test that pushes |

Second part — F-0033 made whole: the sample product's tick in CI walks the failure paths too (held → correct
→ land; unpushed → correct; origin moved → rebase) on the fake runtime, asserting the tick's tables.

Measured: Bugs per landing and rounds per landing on the scorecard, per week.
