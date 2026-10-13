# 0.3.0 ends working cloud sessions for no first beat: the cloud sandbox cannot start the beat
parent: F-0360

Type: Bug
Priority: need

Under v0.3.0 the kernel ends cloud sessions for "no first beat after Nm (past the grace)" although they are working: a cloud session cannot start the heartbeat (earlier sessions reported the beat command refused by the cloud sandbox), so no beat ref ever reaches origin. In the two hours after 0.3.0 was installed on 2026-10-13, 8 of 26 cloud sessions were ended this way (every build or spec that ran past the grace), 0 of 26 had a beat ref, and nothing merged. Sessions that finish inside the grace are unaffected, which is why reviews still completed. The console set kernel.heartbeat.enabled: false as a stopgap.

Acceptance:
- A cloud session sends its beat by a means the cloud runtime allows (for example the harness pushes the beat ref on each tool round, or the runtime reports last activity and the kernel reads that), and the launch verifies the first beat mechanism is available before relying on it.
- A session whose runtime cannot beat is verdict `unknown` and judged by its bound as before, never `no-first-beat`.
- `no-first-beat` ends a session only when a beat mechanism was confirmed for that runtime.
- Test: a cloud session with no beat and progress on its branch is not ended at the grace; a cloud session with a confirmed mechanism and no beat is.
- After the fix, kernel.heartbeat.enabled is set back to true for the product and a 2-hour window shows cloud builds finishing.
