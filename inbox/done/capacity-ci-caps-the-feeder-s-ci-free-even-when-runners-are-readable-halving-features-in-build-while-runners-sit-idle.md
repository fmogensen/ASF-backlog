→ F-0319

# capacity.ci caps the feeder's ci_free even when runners are readable, halving Features in build while runners sit idle

Severity: S2

`capacity.ci` is read two ways. The CI queue applies it only when the runners can't be read (`ci_queue.ceiling_gate`, asf/ci_queue.py:2306). The feeder caps `ci_free` at it even with a live runner read (`asf/capacity.py:333`, `min(free, capacity.ci)`). At or above that many runs in flight, `ci_free` becomes 0, and the auto Features-in-build cap is halved (`asf/feeder/rows.py:555`). Botseon has `capacity.ci: 4` beside a 23-runner pool and showed "ci 4 … free 0 (overrides ci.pool 23)", which throttles Feature parallelism while most runners are idle. `asf status` says the opposite: "ceiling 4 only when they are unreadable".

## Acceptance
- With a runner read available, `ci_free` is the runners' free capacity and is not capped by `capacity.ci`; the ceiling stands in only when the read is None, as in the queue. A test covers it: `capacity.ci: 4`, a pool with 20 free, 6 runs in flight → `ci_free` 20 and the Features-in-build cap is not halved.
- `asf capacity` and `asf status` describe the same rule. A doctor line warns when `capacity.ci` is below the declared pool's slots.
