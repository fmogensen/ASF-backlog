# Intake: an S1 card stuck on a signature question sits silently; the 'S1:' title prefix is ignored

Two botseon S1 inbox cards sat all night on an intake `## Question` ("add signature: …"), and nobody saw it. One of them blocks prod: flaky p1-e2e on main. The groom "ruled 6" that morning, but neither card became a Bug.

Gaps (asf/groom/inbox.py process_inbox):
1. Severity is read only from a `severity:` header. A title prefix `S1:` / `S2:` is ignored, so a card with it would come in as S3.
2. An intake question on a card that says S1 (by header or title prefix) is silent. It should surface in status as NEEDS OPERATOR (or go to the groom adjudicator the same tick) and not wait for someone to open the inbox.
3. Consider accepting a card whose body has an `Error:` / failing-spec line as its signature, not only a `signature:` header.

Tests: an `S1:` title prefix → severity S1. An S1 card left on a question → a status/NEEDS OPERATOR line naming the file.
