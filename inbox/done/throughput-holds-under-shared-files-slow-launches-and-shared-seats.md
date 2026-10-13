→ F-0361

# Throughput holds under shared files, slow launches and shared seats
parent: E-0007

Type: Feature
Priority: need

## Description
Throughput holds under shared files, slow launches and shared seats. On 2026-10-12/13 up to nine Ready items were held by file overlap on four kernel hub files, one tick ran 40 minutes on slow cloud launches while landings and verdicts waited, and the host-wide seat share between products would have cut this product from 8 seats to 2. Takes over S-176462 (hub split) from F-0352.

## Acceptance
- [ ] The four kernel hub files (decide, ports, model, loop) are split along their existing seams so that two kernel Tasks on different concerns no longer share a write path; no behaviour change, proved by the existing suite.
- [ ] A tick is bounded: landing, verdicts and Done marks are applied before launches; launches run under a time budget; a tick that exceeds a configurable limit is recorded as a breach with its slowest step.
- [ ] Seat share between products on one host: a product never drops below a configured floor while it has Ready work, unused share is lent within one tick, and `asf kernel status` shows the share and why.
- [ ] Stacked work: an approved stacked PR waits by name on its predecessor and lands after it without a manual step; a rebuilt predecessor carries its stack along instead of orphaning it.
- [ ] Measured over 24 hours on at least 8 usable seats: seats busy at least 70% while work is Ready, lead time p50 at most 1.0 h and p90 at most 4.0 h.
