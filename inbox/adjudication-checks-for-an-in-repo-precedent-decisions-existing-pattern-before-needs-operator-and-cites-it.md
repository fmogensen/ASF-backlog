# Adjudication checks for an in-repo precedent (decisions, existing pattern) before NEEDS OPERATOR, and cites it

Parent: E-0001

An adjudication session reaches NEEDS OPERATOR on a question the product's own code and decisions already answer. Seen 2026-10-06 on botseon: adjudicate-t-42278 rightly reverted a raw credential read in a web app, then parked the Task on the operator. But the repo already routes that kind of call through a job, queued by the web app and claimed by a worker that holds the credentials under recorded decisions. The ruling followed directly from that existing pattern. Most such parks are avoidable.

## Acceptance
- The adjudicate brief (and the review and correction briefs) requires one step before any NEEDS OPERATOR: search the product's decisions (`decisions/`) and code for an established pattern that resolves the question. When one exists, the ruling cites it (decision id, or file and symbol) and rules on it instead of parking.
- A NEEDS OPERATOR line must say "no precedent found", with what was searched. A line without it is refused by the report parser and goes back to the session once.
- The money, credentials and irreversible classes still go to the operator only when no precedent covers the exact action. Re-using a component that already holds the credentials is not "touching credentials".
- Tests: the brief carries the step; the parser refuses a NEEDS OPERATOR with no precedent statement; a fixture with a matching decision yields a ruling citing it.
