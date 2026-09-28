→ F-0227

# An ended worker run whose pid stays alive blocks its item forever (T-0196)
parent: E-0001

The job review-t-0196 (asf product) finished its output, but its `claude -p` process (pid 67546, a worker session launched by the factory, parent pid 1, 0.1% CPU) was still alive 54 minutes later.

Each tick's health step logged `keep review-t-0196 ended: pid still alive`, and the wave then logged `waits review-t-0196 T-0196 — already running`. So T-0196 was blocked on every tick, with 0 of 2 sessions in use and the item ready to launch. Status showed "0 working" while the wave treated the job as running.

Wanted:
- A run that has ended and whose pid stays alive past a grace period (for example 5 minutes after its result line) is treated as finished.
- The factory terminates that worker process. It should only ever terminate `claude -p` workers it launched itself, never an interactive session.
- The item is released for its next row.
- Add a test covering an ended run with a live pid.
