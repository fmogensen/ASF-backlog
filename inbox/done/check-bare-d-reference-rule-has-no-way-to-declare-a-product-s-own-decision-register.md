→ F-0124

# check: bare D-reference rule has no way to declare a product's own decision register

`asf check`'s "bare decision reference" rule treats any `D<n>` token in prose as a reference to a
record D-card and asks for `[[D-nnnn]]`. A product can have its own decision register in its code repo
(e.g. docs/decisions with D1…D300 rows) that plans and Task bodies cite as "D69/SITE-4" or "the
dev-deploy plan's D267". Rewriting those to `[[D-0069]]` would link to the wrong thing (a different
record card, or none); the only safe workaround is inline code.

Expected: a product can declare its own register — e.g. `conventions.external_decision_prefix: "D"`
or `conventions.decision_register: docs/decisions` — and the rule then accepts bare `D<n>` tokens
(or only flags ones that resolve to a record D-card id). Default behaviour unchanged.
Acceptance: with the convention set, "D69/SITE-4" in a Task body yields no finding.
