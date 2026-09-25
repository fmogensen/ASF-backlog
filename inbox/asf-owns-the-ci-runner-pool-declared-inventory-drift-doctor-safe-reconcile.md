# ASF owns the CI runner pool: declared inventory, drift doctor, safe reconcile

Feature: ASF owns the CI runner pool — declared inventory, drift detection, safe reconcile.

Incident 2026-09-25: 3 of 5 Contabo heavy boxes (8c/24GB) sat idle for days because their labels (`contabo-heavy`) didn't match what every job's `runs-on` required (`hetzner, heavy`). Nothing reported it, while 170 jobs queued. Fixing it needed hand label edits.

1. Declared pool (operator config): `ci.pool:` entries {runner/box, provider, size, role: heavy|light, labels}. Single source of truth; no hand edits on the CI host.
2. Role-based routing: jobs ask for a role label (heavy/light), never a provider label. Doctor flags a workflow `runs-on` that names a provider label.
3. Doctor drift checks: a runner online but no job's labels match it (stranded); a job label set no runner satisfies; runner labels differing from its declared role; a runner offline.
4. `asf ci reconcile`: applies the declared labels idempotently. A newly enabled runner gets a trial of one job before full enablement; red rolls back and files a Bug.
5. `asf capacity` derives the CI limit from the declared heavy and light slots (replacing hand-set capacity.ci).
6. Provider-neutral: GitHub runner labels are the first backend; the same pool feeds `ci.provider: vm` (inbox: external CI on any VM).

Acceptance: tests with a fake runner API covering stranded-runner detection, reconcile adding labels, trial pass and rollback, and capacity from the pool.
