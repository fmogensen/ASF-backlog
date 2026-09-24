# briefs: spec/plan templates don't state the commit-subject rule harvest enforces

Harvest's lane_refusal/commits_name_item (asf/harvest/harvest.py) requires every commit subject on a
lane branch to name the item as a token (`(?<![\w-])F-0007(?![\w])`), and raises a `naming`
correction otherwise. The spec and plan briefs (asf/briefs/templates/spec.md, plan.md) never tell
the session this; only fix-bug.md does ("Every commit subject names the card — `fix({item_id}): …`").
evidence.py's DOC_LANE_SUBJECT expects `spec(`/`plan(` subjects too.

Seen: a plan session committed `docs(brand): BRAND-1 plan on cloud/plan-F-0007 — …`. The only F-0007
is inside the branch name, which the lookbehind correctly rejects, so the finished plan got a naming
correction and a second round instead of landing.

Expected: spec.md and plan.md (and review/adjudicate) state the subject convention, e.g.
`plan({item_id}): <what>` / `spec({item_id}): <what>`, matching DOC_LANE_SUBJECT.
Acceptance: the brief-build test asserts every document-lane template names the subject form;
a plan session following the brief passes lane_refusal on the first round.

## Question
Which Epic is this under? No open Epic shares a title word with it.
