# a stuck item blocking many rows just sits: no blocker weight, no escalation, no top-blockers view; incomplete deliveries park the lead

Severity: S1

Reported by botseon on 2026-10-08, and the same shape stalled ASF today. ASF parks or relaunch-caps an item without regard to how many rows wait on it. At 21:10, 125 of botseon's 273 rows were "WAITS ON <item>", concentrated on a few stuck heads:
- T-0517 is PARKED as "delivery incomplete 2× (still unbuilt: T-0522)". 12 rows wait on it directly, 18 with T-0547's chain.
- T-0422 is at the daily relaunch cap of 6, holding 7 rows.
- T-58724 holds T-58741 and 14 rows.

These sat for hours with cloud at 2–9 of 32. On ASF the same day, ranked Tasks of F-0301, F-0306 and F-0312 sat behind parked holders.

## Acceptance
- **Blocker weight:** the feeder orders rows by a key that includes the number of open rows transitively waiting on the item, after severity and before rank, so an item holding N ≥ 3 rows launches ahead of equal-rank work. A test covers it: two equal-rank Tasks, one with 5 waiters → that one launches first.
- **Escalation:** a parked, relaunch-capped or "needs input" item that blocks 3 or more rows gets an adjudicate row at once, with no wait for the daily cap to reset. Its brief names the blocked count and the waiters, and the session uses the strongest configured model. A test covers it: a capped item with 3 waiters → an ADJUDICATE row with `blocked: 3` in the brief.
- **Visibility:** `asf next` and `asf status` open with a "top blockers" block listing each blocking item, its blocked count and why it's stuck, for the top 5. A test covers the render.
- **Incomplete delivery:** when a delivery is incomplete and a member is still unbuilt, the next pass undelivers that member and lands what was built, instead of parking the lead. A test covers it: a lead whose member never built → member undelivered, lead lands its built members.
