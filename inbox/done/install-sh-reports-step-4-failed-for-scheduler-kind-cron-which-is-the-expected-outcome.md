→ B-0112

# install.sh reports step 4 FAILED for scheduler.kind cron, which is the expected outcome
signature: tools/install.sh reports FAILED step 4 because asf scheduler install exits 3 on scheduler.kind cron, which is its expected outcome
parent: E-0001
severity: S3

With `scheduler.kind: cron`, tools/install.sh step 4 (`asf scheduler install`) exits 3 and the installer reports "FAILED step 4" even when that is the expected outcome for cron (ASF prints the crontab lines for the operator instead of installing them). Found while writing the user guide.

Fix: the scheduler install gets a distinct exit code for "printed the lines for the operator", and the installer reports it as "ACTION: add these crontab lines", not as a failure. Or the scheduler install writes the crontab entries itself, idempotently, between markers.

Test: kind cron → the installer exits 0 with the action line; a real failure still exits non-zero.
