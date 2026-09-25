→ B-0133

# asf itself must use remote CI: no full suite on the host from workers or harvest

type: bug
severity: S2

Operator, 2026-09-25: "please use remote ci from now on". The asf product still runs every test on the factory host: each worker runs the full suite before pushing, and harvest's fast-forward landing runs it again in product_gate. That held load at 30-78 for hours today and forced asf down to 1 session. This supersedes the earlier card "Every asf worker runs the full suite that harvest's gate already runs". Want: the ASF repo's suite and lints run in remote CI (a GitHub Actions workflow on pull requests: tools/run_tests.py, check_generic, check_conventions), and the asf product lands by pull request with `landing_checks` naming that job and `landing_checks_missing: wait`, with `landing_checks_wait_min` high. Its `test_command` is "" (never `none`, which 0.1.x executes as a command). Workers then get the existing external-CI brief rule (targeted checks only). Also fix the trap itself: `test_command: none` / `off` should mean no local command, and doctor should flag a test_command that is not an executable. Tests: an asf-like product with PR landing and landing_checks gets the external-CI brief rule and a product_gate with no command; `test_command: none` loads as no command.
