→ F-0316

# groom drops an inbox note's Acceptance and Severity lines when it creates the card

Severity: S2

Groom turned inbox note `delivery-pass-binds-…` into F-0315 (2026-10-08 15:10Z). It copied the note's body into `## Description` but left `## Acceptance` as an empty `- [ ]`, and dropped the note's four acceptance lines and its `Severity: S1` line. A defect filed with acceptance therefore reaches the factory with none, and a reviewer has nothing to prove against. Restored by hand on F-0315.

## Acceptance
- An inbox note with an `## Acceptance` section becomes a card whose `## Acceptance` holds the same lines, in order. A test covers a Feature-shaped note and a Bug-shaped note.
- A note's `Severity: S1|S2` line becomes the card's severity, and a note carrying one is shaped as a Bug unless it says otherwise.
- Groom refuses, by name, to write a card with an empty Acceptance when its note had one.
