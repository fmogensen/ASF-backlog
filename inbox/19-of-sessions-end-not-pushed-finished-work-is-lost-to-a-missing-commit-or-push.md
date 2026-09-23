# 19% of sessions end 'not pushed': finished work is lost to a missing commit or push
parent: E-0002

Of 229 ended sessions in the asf ledger, 43 (19%) ended `failed: not pushed: N uncommitted file(s)` and 4 ended `failed: unpushed work`. The coder finished its work and exited without committing and pushing it. Each one costs a full relaunch (a correction session), and the same Task often fails the same way two or three times (T-0026, T-0027, T-0037, T-0040, T-0048).

Expected: work a session did is never lost to a missing commit or push. Either the session's end hook commits and pushes on the session's behalf, or the brief and the runner enforce it before the session may exit. The failure rate should also be a tracked metric that fails a threshold.
