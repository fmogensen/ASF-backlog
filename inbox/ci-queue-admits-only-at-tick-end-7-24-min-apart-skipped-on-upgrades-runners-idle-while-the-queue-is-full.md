# CI queue admits only at tick end (7–24 min apart, skipped on upgrades): runners idle while the queue is full

2026-09-26 22:37–22:43 botseon: 7 heavy + 7 light runners idle while the CI queue held 45 and its head showed "would start". Admission only happens in the lane pass inside the tick's harvest step, i.e. once per tick at its END — botseon ticks run 7–24 min (record 205 s, health 400–900 s, wave 400 s), and harvest is skipped whenever an upgrade is pending ("harvest: not started — upgrade … pending"). So idle runners wait up to a whole tick or two.

Fix: run the CI queue pass (admission, relief, starvation guard, dedupe) on its own short cadence, independent of the tick — its own scheduler job every 1–2 min with its own lock (e.g. `asf ci queue --apply` via launchd), or at least at tick START as well as end. It must not wait on or be skipped by an upgrade drain (it is a quick, idempotent pass). Test: admission runs while a tick is mid-health; drain doesn't skip it.

## Question
Which Epic is this under? No open Epic shares a title word with it.
