# Every product console shows the FACTORY STATUS table every 5 minutes, by code

type: bug
severity: S2
after: [B-0087]

Operator, 2026-09-24: "Why don't you show 5 min status ticks? Should be part of asf." Every product console should get the FACTORY STATUS table (/asf:status: in flight, done since, waits and holds, bottleneck) automatically every 5 minutes. No session should have to remember to start a loop; today each console runs `/loop 5m /asf:status` by hand. This is the console half of B-0087 (the per-tick digest and `asf watch`, decided 2026-09-24), which already asks the generated plugin to wire `asf watch` into the console.

Want: the plugin (asf/plugin_build.py) ships the mechanism in code: a SessionStart hook or a declared monitor, whichever the plugin docs support. It starts a 5-minute status feed in every console where ASF_PRODUCT or default_product names a product, printing the FACTORY STATUS table and the tick digest deltas since the last print. The interval is configurable (operator, 2026-09-24: "Configurable"): `console.status_every` in config.yaml, overridable per product in products/<p>.yaml; default 5m; `off` (or 0) disables it. No per-session rule and no remembered loop. Tests cover the generated plugin carrying the hook, and the feed printing the table on a fixed clock.
