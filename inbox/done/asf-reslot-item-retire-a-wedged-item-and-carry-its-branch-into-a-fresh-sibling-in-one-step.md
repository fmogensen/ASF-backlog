→ F-0263

# asf reslot <item>: retire a wedged item and carry its branch into a fresh sibling in one step

Parent: E-0001

When an item's lane state is wedged (a stuck verdict, a correction the lane cannot see, a looping rewrite), the only escape is a hand sequence: retire the item, close its PR, create a fresh item with the same writes, and write a brief that resets to the old branch. Seen 2026-10-06 on botseon: T-42278 was retired and T-44931 created to carry cloud/T-42278 forward.

## Acceptance
- `asf reslot <item> --why <text>` does it in one step. It retires the item ("superseded by <new>"), closes its PR with a comment naming the new item, and creates a sibling under the same parent. The sibling copies title, writes, acceptance, proves lines and links, and adds any `--writes` given. Its branch starts at the old branch's origin head, so no commit is lost, and the History of both items links them.
- The new item's first launch starts from that head, and the lane carries over no lane state, verdicts or corrections from the old item. Only an operator ruling given with `--carry-ruling` is copied.
- Refused for an item with a live session, or one already landed. A dry run (`--dry`) prints the plan.
- Tests: reslot of a wedged PR_OPEN item yields a launchable sibling at the same head and a retired original. A live session is refused. A landed item is refused.
