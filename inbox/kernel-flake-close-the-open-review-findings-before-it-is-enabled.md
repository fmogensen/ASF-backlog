# kernel.flake: close the open review findings before it is enabled
parent: F-0336

Type: Bug

kernel.flake (T-136825, PR #1657) was landed by the console after 4 fix rounds with open review findings, because the module is off by default (kernel.flake.enabled: false) and each round surfaced new edge cases. These must be closed before any product turns kernel.flake on.

Open findings in asf/kernel/flake.py judge():
- The bugless-live-entry path (around :359-372) must call owned(sig, facts) and file no second card when another open Bug owns the signature.
- A Bug filed on that path must carry the test file in writes: and an acceptance line naming the test. With no test file in the log, file nothing and record "no test file in the log: nothing filed" (also :329-332 for case-keyed signatures).
- An entry whose bug is empty is filled from cycle(sig, facts) unconditionally, not only when the merged set grew.
- tests/kernel/test_flake.py: add the PD5 assertion (a live entry is live for every matrix leg in asf.flake.active but exact in decide.quarantined) without clearing the quarantine.

Acceptance:
- Each finding above has a test that fails before and passes after.
- A check refuses kernel.flake.enabled: true in a product yaml until this Bug is Done, or the default stays false with a note naming this Bug.
