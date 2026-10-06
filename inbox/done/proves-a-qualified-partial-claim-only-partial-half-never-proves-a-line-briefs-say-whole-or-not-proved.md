→ F-0257

# Proves: a qualified/partial claim ("only", "partial", "half") never proves a line; briefs say whole or Not proved
parent: E-0001

From a product session (2026-10-06): a coder wrote `Proves: S-0021 line 6 — …guard.test.ts (type guard, size guard, EXIF strip only)` in a PR body while its tests asserted only part of line 6; under "no test, no done" that would have ticked the whole acceptance line. Fixed by hand in the PR body.

## Acceptance
- [ ] `asf.proves.parse` rejects (does not count) a `Proves:` claim whose text carries a partial qualifier — "only", "partial", "partly", "half", "except", "not yet", "minus" (list configurable, `proves.partial_markers`) — and returns it as a refused claim with the reason.
- [ ] The landing/evidence path reports a refused partial claim as a finding ("line not proved: partial claim"), never as proven; review-checks flags it on the PR.
- [ ] The coder and correct briefs say: a line is proved whole, or it goes under "Not proved" with what is missing — never a qualified Proves line.
- [ ] Hermetic tests: qualified claims refused, whole claims accepted, existing Proves fixtures unchanged.
