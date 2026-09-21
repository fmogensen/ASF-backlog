# Closing is derived and total: a definition of done per type, reconciliation for work that predates it, nothing re-emitted
type: feature

## Description
As an operator, I want every item to close by itself the moment its work is demonstrably done — and
work done before the marker existed to be closed through one groom question — so that the factory
never develops the same thing twice because a card stayed open.

Today: a Bug/Task turns `Resolved` on a trunk commit naming it and `Closed` on green CI after it; a
Feature lands when its children close or a commit names it. Gaps: a Story with no Tasks, a Feature
with no children and no naming commit, and everything landed before the id-in-subject convention
(2026-09-21) stay open forever; the feeder re-emits an open Bug every wave with no attempt cap
(B-0026); an operator names nothing by hand (a typed status lies).

## Acceptance
- [ ] Definition of done, per type, in `docs/` and in code (`asf.record.ingest`): **Bug/Task** —
  trunk commit naming it → Resolved; CI green at/after it, or `ci: none` → Closed. **Story** — all
  Tasks Closed, or (no Tasks) a PR whose body ticks every acceptance line and is merged → Closed.
  **Feature** — all children Closed, or a naming trunk commit → `landed`; the deploy sha contains
  the landing commit, or `deploy_sha: none` → `on-prod`/Closed. **Epic** — typed `closed` by the
  operator; the tick proposes it in the groom when every Feature is Closed. **Decision/Rule** —
  never close; `superseded_by` instead.
- [ ] `asf reconcile --product <p>`: for every open Feature/Story/Task/Bug, matches the card's
  title and `## Fix`/`## Acceptance` terms against trunk commit subjects and merged PR titles since
  the card's creation; a match above a threshold becomes a groom line `close? <id> — <n> commits
  match: <sha> <subject>`; the operator's `yes` writes `closed_by: <sha>` (typed — a ruling with its
  evidence) and the tick derives Closed from it. Never closes anything on its own.
- [ ] Every brief kind requires the item id in every commit subject (`<kind>(<id>): …`); harvest
  refuses a branch whose commits do not name the branch's item.
- [ ] The feeder emits no row for an item in a done state and caps attempts (B-0026); `asf next`
  prints `done: <n> items closed since the last tick`.
- [ ] A test per rule above on the `sample/` product; a test that a card closed by `closed_by`
  never re-emits.

## History
- 2026-09-21 19:45 operator: "if we don't close stuff when it's done, we'll keep developing the
  same things in loops endlessly"
