→ F-0346

# Flow conservation: every step has a required successor, checked each tick
parent: E-0003

Flow conservation: every step has a required successor within a bound, checked every tick (operator-approved 2026-10-10, ASF 0.3).

## Problem
On 2026-10-10 four pipeline gaps were silent for hours: the kernel never minted Tasks from merged plans (22 plans, 0 Tasks); a Feature went Done when its plan merged; plan-minted Tasks lost Acceptance to a 4000-char cut; the repo checkout was 131 commits stale so landed specs were invisible. None was a slow step — each was a step that never happened, and LIMBO did not see it because the item looked finished or idle-by-design.

## Change
A table of conservation rules in kernel code — (precondition, expected successor, bound) — evaluated each tick over facts, with every rule's bound a product-yaml knob under `kernel.flow`:
- spec PR approved -> plan session launched or plan PR open (1 tick)
- plan merged -> the plan's Tasks exist on the record, or a replan/refusal is noted (1 tick)
- Task minted from a plan -> passes DoR or carries a noted DoR reason (1 tick)
- PR open -> required CI runs exist on its head (bound from the ci p90)
- Feature Done -> every Story under it Done and every Acceptance test named on its Stories/Tasks present on main
- record/trunk reads are no older than origin/main at tick start
A violated rule is a Stuck/LIMBO row with the rule name as its reason, and the kernel applies the known repair where one exists (mint, relaunch, re-read). Rules are listed by `asf kernel flow --product P` with pass/violated counts.

## Acceptance
- Each of the four 2026-10-10 failures, replayed as a fixture, is reported by the named rule on the first tick (test per rule).
- A Feature cannot reach Done while a Story under it is open or an Acceptance test it names is absent on main (test).
- `asf kernel flow` lists every rule with its bound and current violations (test).
- Zero violations on the live record after install, or each violation is a real defect filed (verified at install).
