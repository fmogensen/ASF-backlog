→ F-0353

# Session kinds run on the cheapest model that does the job (per-kind model map)
type: feature
parent: E-0006


Session kinds run on the cheapest model that does the job. Measured 2026-10-11: build, intake-decide and groom-fill ran on claude-sonnet-5, and plan sessions on claude-opus-5. Intake-decide and groom-fill (261 of 1471 launches) are classification tasks with a small structured verdict.

## Change
- A product-yaml map `kernel.models: {<session kind>: <model label>}` sets the model per session kind (seed: intake-decide and groom-fill → haiku; review → sonnet unless high-risk, which stays on strong_model; build and fix → sonnet; spec and plan → opus).
- The strong model is used only on escalation (round 3 or later, or high risk), as today.

## Acceptance
- Each session kind launches on the model the map names, and an unmapped kind keeps today's default (test).
- `asf kernel status` or the waits view shows sessions per model per day, so the cost effect is measured.
