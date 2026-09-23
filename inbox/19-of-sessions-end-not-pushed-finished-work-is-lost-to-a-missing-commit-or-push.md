# 19% of sessions end 'not pushed': finished work is lost to a missing commit or push
signature: failed: not pushed: N uncommitted file(s)
parent: E-0002

Of 229 ended sessions in the asf ledger, 43 (19%) ended `failed: not pushed: N uncommitted file(s)` and 4 ended `failed: unpushed work`. The coder finished its work and exited without committing and pushing it. Each one costs a full relaunch (a correction session), and the same Task often fails the same way two or three times (T-0026, T-0027, T-0037, T-0040, T-0048).

Expected: work a session did is never lost to a missing commit or push. Either the session's end hook commits and pushes on the session's behalf, or the brief and the runner enforce it before the session may exit. The failure rate should also be a tracked metric that fails a threshold.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.

Update 2026-09-23 20:00: F-0089 ("No run ends unlanded: the end-of-run check happens inside the session") landed at 18:55, but sessions launched after it still end the same way. coder-t-0055 started about 19:28 and ended `failed: not pushed: 5 uncommitted file(s)`. On the 20:00 tick alone, T-0043, T-0032, T-0045, T-0034 and B-0076 each needed a correction session for the same reason. Either F-0089's check is not in the path coders take, or it runs but does not commit and push. Root-cause against F-0089's change.
