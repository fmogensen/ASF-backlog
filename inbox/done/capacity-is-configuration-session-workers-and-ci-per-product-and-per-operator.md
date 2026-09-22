→ F-0079

# Capacity is configuration: session workers and CI, per product and per operator
parent: E-0001
type: feature

## Description
As an operator, I want to set how many sessions and how many CI runs the factory may hold at once —
per product, and as a total across products — so that a product's share of the workers and the
runners is a line in a file, not a number typed into a launch script.

Today: `config.yaml feeder.capacity` is one number for sessions; accounts carry `cap`; the S1
reserve is `worker_pool.reserve_for_s1`; CI capacity is whatever the product's merge-queue script
hardcodes (batches per run, parallel batches, runner count).

## Acceptance
- [ ] `config.yaml`: `capacity: {sessions: {total: 8, local: 4, cloud: 4}, ci: {total: 6}}` — the
  operator-wide ceilings; `worker_pool.accounts[].cap` stays the per-account ceiling.
- [ ] `products/<p>.yaml`: `capacity: {sessions: 2, ci: 2, reserve_for_s1: {local: 1, cloud: 1}}`
  — the product's share; the wave never exceeds `min(product.sessions, total − other products' in
  flight)`; the `batch` step never holds more than `product.ci` runs in CI at once.
- [ ] `asf next --product <p>` prints the capacity line it planned against (`capacity 2 of 8 · 1 in
  flight · 1 free`) and the reason a row waits (`reserved for S1`, `product cap`, `total cap`,
  `account cap`, `quota`).
- [ ] The CI provider interface gains `in_flight(product) -> int` (gh-actions: queued + in-progress
  runs on the product's repo, filtered by the batch workflow); the `batch` step reads it and the
  product cap before cutting.
- [ ] `asf status` shows sessions and CI as `used / cap` per product and total; `asf doctor` flags a
  product whose cap exceeds the total.
- [ ] Changing a cap needs no restart: every tick reads the yaml.

## History
- 2026-09-21 19:42 operator: "capacity should be configurable for session workers and for CI"
