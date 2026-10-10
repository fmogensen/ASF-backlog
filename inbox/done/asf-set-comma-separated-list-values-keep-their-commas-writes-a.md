→ B-121288

# asf set: comma-separated list values keep their commas (writes: ["a,", ...])
signature: asf set keeps commas in list values S3

Bug: `asf set <ID> writes="a, b, c"` splits on whitespace and keeps the commas, writing `writes: ["a,", "b,", c]`. The paths then never match the boundary and the session brief shows a broken writes list (seen 2026-10-10 on T-0196 and T-0123; both repaired by hand with a space-separated value).

Acceptance: list fields given to `asf set` accept comma- and/or space-separated values and never store an entry ending in a comma; a test in tests/ covers `writes="a, b"` and `writes="a b"` producing `[a, b]`.
