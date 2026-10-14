→ B-226064

# A hung local session keeps beating through the host loop and is ended only at the 6 hour ceiling
parent: F-0360

Type: Bug

A local session whose model hangs while its pid lives keeps beating: the host loop in asf/workers/heartbeat.py (start_loop) pushes a new sha every interval, so verdict() never reads frozen, and only the 6 h max_age ceiling ends it. RealSessions._log_idle_min in asf/kernel/ports.py already reads the job log's age, but only to keep a no-first-beat session alive.

## Acceptance
- [ ] A beating local session whose job log has not changed for kernel.heartbeat.log_silent_min is ended as frozen, proved by a test.
- [ ] A session whose log keeps moving is not affected.
