# asf config check/migrate: an old or hand-made product yaml maps onto the schema without a hand rewrite

A product yaml written against an older or hand-made shape turns doctor RED with no path forward except a hand rewrite. Keys with no home in the schema (a secrets store, CI budgets, runner pools, spec/plan approval policy) get lost or kept as comments. Reported by the first customer install.

Want: `asf config check --product P` and `asf config migrate --product P`.
- check: lists every unknown or renamed key, with the current key it maps to, or "no home: see <card>".
- migrate: rewrites known renames in place, keeps a backup, never drops an unknown key (moves it under `x-unmapped:` with a comment), and is idempotent.
- doctor points at `asf config check` when the config fails to load.
- Unknown keys are reported, never a crash. Covers the prod-view crash on a list-shaped `customer_paths`.

Tests: rename mapped; unknown key kept under x-unmapped; second run is a no-op; doctor hint.
