# Self-tuning loop v1: per session kind, model and seat share tuned from measured outcomes, within operator bounds, auto-revert

Operator 2026-10-06: "yes, include the self-tuning loop in the release" — ASF's goal is a self-optimizing autonomous software factory; today it self-improves only by filing cards (scorecard loop). The first public release must have ONE closed, guarded self-tuning loop.

Scope v1: per session kind (coder, review, correct, spec, plan, …) ASF tunes (a) the model and (b) the seat share, from measured outcomes.
- Signal: per kind, over a rolling window (`tune.window_days`, default 7, min `tune.min_samples` 10): repair rounds per landed item, landed share, $ or quota-% per landed item, wall time.
- Policy (deterministic code, no LLM): try one step at a time (cheaper model, or ±1 seat) for a kind; keep it if the objective (landed items per quota-% at repair ≤ baseline) improves by ≥ `tune.min_gain` (10%) after `tune.trial_samples`; revert automatically if repair or failure rate worsens beyond `tune.max_regress` (10%).
- Guardrails: operator bounds per knob (`tune.bounds.<kind>.models: [...]`, `seats: [min, max]`); never outside them; at most one live trial per kind; `tune.enabled` (default false for products, true for ASF itself); `asf tune freeze` stops all trials.
- Every change is a ledger event and a line in `asf status` ("tune: review sonnet→haiku trial 4/10, repair 1.1 vs 1.2"); `asf tune history` shows changes, reasons, outcomes; reverts are logged with numbers.
- Generic: model names come from config/runtime connector, not code; works with one account and no cloud.
Acceptance (hermetic, fixture session streams): a kind whose cheaper model keeps repair flat gets switched; one whose repair rises is reverted; bounds respected; freeze stops trials; no change with < min_samples; ledger/status/history rows rendered.
Release: add criterion 11 "Self-tuning live — ≥ 1 kept change and 0 unreverted regressions in the window" to `asf release-readiness`.
