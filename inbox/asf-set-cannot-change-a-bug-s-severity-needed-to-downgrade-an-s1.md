# asf set cannot change a bug's severity (needed to downgrade an S1)

2026-09-26 20:35: the botseon session needed to downgrade B-1382 S1 → S2 (main green without it, prod deployed) to release the S1 lane's hold on 8 Ready rows; `asf set B-1382 severity=S2` refused ("no settable field 'severity'"), so it was hand-edited in the record. Make `severity` settable via `asf set` for bugs (S1/S2/S3 validated), appending a History line "severity S1 → S2: <--why>" and requiring --why for a downgrade from S1. Test: set severity on a bug writes the field + history; invalid value refused.
