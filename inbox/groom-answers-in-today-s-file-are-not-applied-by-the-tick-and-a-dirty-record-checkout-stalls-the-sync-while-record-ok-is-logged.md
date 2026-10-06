# Groom answers in today's file are not applied by the tick, and a dirty record checkout stalls the sync while 'record ok' is logged

Parent: E-0001
severity: S2

The "NEEDS OPERATOR" row tells the operator to answer an inbox question in groom/<today>.md and says "the next tick applies it". It doesn't. The tick's groom step applies only the previous day's file, so today's answers wait for tomorrow or a hand `asf groom --apply`. Answering the line also left an uncommitted edit in the record checkout. From then on the record step stopped syncing, yet logged `record ok`: the checkout fell 28 commits behind origin in about an hour. A hand `groom --apply` on that stale checkout then minted B-0268 to B-0270, which collided with different Bugs already pushed under those ids. Seen 2026-10-06 on ASF's own record.

## Acceptance
- The tick's groom step applies answered lines from today's groom file as well as the previous day's, so the "next tick applies it" remedy is true. Tested.
- An operator edit to a groom file in the record checkout is committed by the record step and pushed (rebased onto origin) before the pull, and never silently blocks the sync. Tested with a dirty groom file.
- The record step reports `record behind N` (not `ok`) when the checkout is behind origin after its sync. A watchdog breach fires when that lasts more than 2 ticks. Tested.
- `asf groom --apply` refuses to mint ids on a checkout that is behind origin (it fetches first), and names the remedy. Tested.
