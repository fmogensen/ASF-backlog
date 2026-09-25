# Feeder: finish planned Features before starting new specs

Feature: the feeder finishes Features before it starts new ones.

Evidence (2026-09-25, botseon): specs and plans land within hours, but code Tasks stall and about 60 stale code PRs piled up. WIP and finishing are the constraint, not spec throughput.

Rule (asf/feeder/rows.py plan_rows, called by step_wave.gated_plan):
- A Task row for an already-planned Feature outranks every spec/plan row.
- While more than N planned Features are still unbuilt, cap spec+plan rows in flight at 2 per product. N is a product setting with a sensible default.
- Deterministic policy, no LLM. Show it in `asf next` so the reason a spec waits is visible.

Acceptance: a test where planned-unbuilt > N, and the feeder picks Task rows first and holds a third spec row.
