# Failed tick step logs no traceback

signature: "a failed tick step logs only the exception message, with no traceback"
severity: S2

## Description
- The tick's wave step failed twice with `[step:wave] FAILED 'str' object has no attribute 'get'`, only on ticks where rows reached launch. The tick log keeps that one line, and no traceback exists anywhere under the logs directory, so the failing reader cannot be located without re-deriving the call path by hand.
- Planning and brief building for the same rows are clean: a dry-run of the step with the launcher and the hold-raiser stubbed returns 0. The fault is therefore in the launch path, which cannot be re-run safely to reproduce it.

## Expected
A failed step writes its full traceback, at least to the tick log, and ideally names the config key being read.
