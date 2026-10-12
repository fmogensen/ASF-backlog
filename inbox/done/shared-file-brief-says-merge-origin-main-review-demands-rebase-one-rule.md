→ B-195073

# Shared-file brief says merge origin/main; review demands rebase: one rule
parent: F-0352

Type: Bug

A build brief gives two contradicting branch rules. The shared-file note added for kernel.overlap.max_holders says "git fetch && git merge origin/main before every push". The review then asks the author to "drop the merge commit: rebase the branch onto origin/main ... never merge origin/main in". On 2026-10-12, T-136825 spent a fix round on exactly this.

Acceptance:
- One rule in every brief and review checklist: rebase onto the base, no merge commits. The shared-file note says rebase.
- Test: a brief built for an item with a shared write path contains no instruction to merge the base in.
