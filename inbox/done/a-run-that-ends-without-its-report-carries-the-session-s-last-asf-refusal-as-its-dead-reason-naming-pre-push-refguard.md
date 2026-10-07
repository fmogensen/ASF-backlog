→ F-0266

# A run that ends without its report carries the session's last ASF refusal as its dead reason (naming, pre-push, refguard)
type: feature
parent: E-0001


A session that ends without its report commit is recorded as "succeeded without the report commit". The reason it stopped is lost, even when that reason was a refusal ASF itself gave: the naming check refusing the push, a pre-push check failing, a protected ref. Seen 2026-10-07 on botseon T-44931: two cloud coder runs ended this way, and the real cause, "commit subjects don't name T-44931", was only found by hand.

## Acceptance
- When a run ends without a report, its dead reason carries the last ASF refusal the session met: the naming check, the pre-push check, a refguard refusal or a push rejection, with that refusal's own text and a stamp. The reason is recorded from the hook or push output, not guessed. It applies to local and cloud runs.
- `asf status`, `asf next` and the relaunch brief show that reason, and the relaunch brief puts the remedy first, for example "subjects must name <id>".
- Two consecutive runs that end with the same refusal are parked with that refusal as the cause, not relaunched blind.
- Tests: a naming refusal in the session yields a dead reason naming it; the same refusal twice parks with that cause; no refusal keeps today's generic reason.
