→ F-0228

# Health does not re-push a hook-refused run until its head or the origin tip moves
parent: E-0002

Health re-pushes a run whose push the product's pre-push hook refused, silently, every tick — running the hook (16–182s on botseon) each time for an unchanged branch.

Evidence (2026-09-28, botseon, measured after #186): `cloud/spec-mobile-pass` costs ~31s per tick this way; publish pushes were 77% (376s of 490s) of a sampled health step before #186 parallelised them.

Want: `publish_gap` / `republish` (asf/workers/health.py) remember the (local head, origin tip) pair of the last hook refusal and skip the retry while both are unchanged; surface the refusal once as a hold with the hook's reason so a session can fix it.

Verify: a test beside tests/test_health_publish_steps.py — a refused push is not retried on the next tick with unchanged heads, and is retried after a new commit.
