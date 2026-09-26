# In-flight table calls cleanly exited sessions 'dead pid'

The tick in-flight table prints "dead pid <age>" for sessions that exited normally with a clean result record and have not yet been re-judged by the next health pass (health.py alive_for). On 2026-09-26 at 09:04, six botseon sessions and one asf session all showed "dead pid 17m". All seven had finished cleanly and were reconciled at the next health pass as finished or failed. The supervisor treated them as dead sessions.

Expected: a session whose pid has exited reads "exited — awaiting harvest" (and "finished" when its log ends with a result record). "Dead" is kept for exits with no result record. The age shows how long ago it exited, not how long ago it launched.

Test: a clean exit with a result record, not yet health-judged → the row reads exited/finished, not dead.
