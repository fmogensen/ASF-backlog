→ closed (groom 2026-10-10, intake intake-decide-inbox.readme-refresh-no-op-test-flakes-across-a-second-boundary-1791645681)

# readme refresh no-op test flakes across a second boundary

`tests.test_sample_product.ReadmeRenderTests.test_refresh_is_a_no_op_the_second_time` is a time-dependent trunk flake (from 6c2cabb5d, T-0140). `asf/views/readme.py:246` stamps `generated` at one-second resolution (metrics.iso) and `write_if_changed` (`asf/metrics/metrics.py:1054`) compares whole bytes, so two back-to-back refreshes that straddle a second boundary rewrite `readme-numbers.json` and `assertNotIn('rewritten', out)` fails. Reproduced 2/15 locally; red on PR #1304 (run 38008654697, tests (3.13)).

Fix: compare the facts ignoring `generated` (or stamp `generated` only when the numbers change). Acceptance: a test that freezes the clock across a second boundary between the two refreshes and asserts the second refresh is a no-op.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.
