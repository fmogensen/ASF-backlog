→ B-157108

# Bug: test_host and test_clean_floor fail locally under the launchd guard
parent: F-0346

type: bug
parent: F-0346

Bug: tests.kernel.test_host and tests.kernel.test_clean_floor fail locally (18 failures) since the launchd guard (#1451). They drive the scheduler under a test HOME, and the guard refuses that correctly. CI passes only because it opts in with ASF_LAUNCHD_FOREIGN_HOME_OK=1. Local suite runs by agents and builders then report false reds.

## Acceptance
- Both modules use the stub launchctl that the suite puts first on PATH, and pass locally with no opt-in (test run, no env var).
- No test needs ASF_LAUNCHD_FOREIGN_HOME_OK; CI drops the opt-in, and the suite stays green.
