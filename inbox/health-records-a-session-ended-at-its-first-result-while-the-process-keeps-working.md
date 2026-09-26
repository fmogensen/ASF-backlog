# Health records a session ended at its first result while the process keeps working

Proven 2026-09-26: asf review-t-0075 (pid 38052) was recorded as ended ("finished") at 13:28Z on its first result record, but the process kept working. Its log grew to 9 results, the last at 13:41Z. The lane moved T-0075 on to pushed/PR while its session was still running, and the seat looked free.

Expected: a session is ended only when its pid has exited, or its final result record is followed by the process ending. A later result record means it is still working.

Test: a result record while the pid is alive → still working.
