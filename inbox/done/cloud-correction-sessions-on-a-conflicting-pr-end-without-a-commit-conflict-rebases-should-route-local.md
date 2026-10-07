→ F-0289

# Cloud correction sessions on a CONFLICTING PR end without a commit; conflict rebases should route local

Parent: E-0001
severity: S2

Cloud correction sessions on a CONFLICTING PR end without a commit. Twice on 2026-10-07 on botseon, correct-t-0064 at 15:57 and correct-t-51408 at 23:27, a cloud correction asked to rebase onto main and resolve a real conflict ended both runs "without the report commit". The relaunch cap then parked them. Run locally (local_only), the same corrections worked. Likely cause: the cloud runtime cannot finish a conflict rebase (an interactive or conflict stop, or a push of a rewritten branch refused for a cloud session), and the failure is silent.

## Acceptance
- A correction of kind `conflict` is routed to a local seat by default (config `cloud.route.conflict: local`), or the cloud brief carries a non-interactive rebase recipe whose result the runtime is allowed to push. Tested on the routing rule.
- A cloud run that ends without a commit on a conflict correction has its dead reason (F-0266) state the rebase stop. After one such end, the item is re-routed to local automatically instead of counting toward the relaunch cap. Tested.
