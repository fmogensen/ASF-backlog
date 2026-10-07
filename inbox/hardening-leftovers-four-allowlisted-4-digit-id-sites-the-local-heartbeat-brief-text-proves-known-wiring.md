# Hardening leftovers: four allowlisted 4-digit id sites, the local heartbeat brief text, proves known= wiring

Parent: E-0001

The hardening pass left four 4-digit-only id patterns, allowlisted in tests/test_id_pattern.py: asf/record/ingest.py:633 (`merged into`), asf/harvest/lane.py:248 (ITEM_ID_RE), asf/workers/runtime.py:770 and asf/scorecard/score.py:30. There is also the local brief text in asf/workers/heartbeat.py that still asks a session to start the beat loop, and lane/evidence not yet passing `known=` to proves.parse_all, so the "unknown story" line shows on PR bodies.

## Acceptance
- All four sites import the shared pattern from asf/record/core.py, and their allowlist entries are removed from tests/test_id_pattern.py, which then passes with an empty allowlist. Tested.
- The local brief never asks for a background beat loop. A brief scan test enforces this.
- The lane and evidence pass the record's known ids to proves.parse_all, and a PR body citing an unminted Story shows "Not proved: unknown story". Tested.
