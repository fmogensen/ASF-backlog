# ASF 0.4 — a day without the console

Type: Epic
Priority: need

## Description
ASF 0.4: the factory runs a full day without the console. On 2026-10-12/13 the 0.3 build needed the console for 111 of 127 questions, about twenty rulings on Tasks stuck after 3 to 7 fix rounds, five kernel fixes made outside the factory, one kernel pause and one retire to break a rebuild loop. 0.4 removes those causes.

## Acceptance
- [ ] A 24-hour run with the console hands-off: `asf kernel gate` is green on every row at the end, and no item needed an operator ruling, a hand merge, a hand install or a kernel pause.
- [ ] Questions resolved by code are at least 80% over that window.
- [ ] 0.4 itself runs as the kernel for that window (self-installed, not installed by the console).

## Non-goals
- New product-facing features. 0.4 is the factory running itself.
