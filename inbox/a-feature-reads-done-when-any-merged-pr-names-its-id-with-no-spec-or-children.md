# A Feature reads Done when any merged PR names its id, with no spec or children
parent: E-0006

Type: Bug

A Feature is derived Done as soon as any merged pull request carries its id, even when the Feature has no spec, no Stories and no Tasks. On 2026-10-13 a docs-only PR (#1695) merged from branch `fix/F-0359-0.4-evidence`; the kernel then read F-0359 as Done with its spec still unpublished on `spec/F-0359`. `asf reopen` refuses (the card is New) and `kernel_reverted` is not settable, so the only way out was to file a replacement Feature.

Acceptance:
- A Feature or Story is Done only when its own children are Done (or it is a leaf with a landed build PR of its own lane); a merged PR on a `fix/` or docs branch that merely names the id does not land it.
- A supported command un-lands a wrongly attributed PR from an item (for example `asf undeliver <item> --pr N`), recorded with a reason.
- Test: a Feature with no children and a merged `fix/<Feature id>-…` PR stays in its spec lane.
