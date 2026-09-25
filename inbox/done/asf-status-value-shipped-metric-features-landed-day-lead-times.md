→ F-0145

# asf status: value-shipped metric (Features landed/day, lead times)

Feature: a value-shipped metric in `asf status`.

Show per product:
- Features landed per day, over 7 days.
- Median card→landed lead time and median Task→merged lead time, computed from the record's landed dates.

For visibility only: priority comes from the finish-first feeder rule, not from this metric.

Acceptance: a status view test with fixture cards that have landed dates.
