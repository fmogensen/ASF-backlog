# Weekly learning review (defect classes seen twice) and a daily per-product scorecard, computed from the record

Parent: E-0001
priority: nice

Stable-core plan, items 11 and 12. A weekly learning review and a daily scorecard, computed by code from the record.

## Acceptance
- `asf review-week --product P` groups the product-reported Bugs filed in the last 7 days by class (module and keyword clusters), and flags any class seen twice or more as a structural-fix candidate. Tested.
- The daily scorecard line per product gives repair sessions per Feature, first-push-green rate, lead time, cost per Feature and the S1 count, with 7-day trends. The release-readiness line reads it. Tested.
