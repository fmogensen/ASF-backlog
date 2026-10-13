→ B-208438

# Intake mint is not atomic: a card stayed an uncommitted file for 30 hours and was never built
parent: F-0360

Type: Bug

Intake minted a card (S-185817, 2026-10-12 06:18Z) and moved its inbox note, but the change was never committed: the card stayed an untracked file and the note a pending delete in the record checkout for 30 hours. The kernel logged "intake: minted S-185817" and "INTAKE S-185817 -> need", so it believed the Story existed; no session ever built it, and the checkout stayed dirty (pulls with rebase refused). The Story was the red-on-base proof, the main fix for review loops. It coincided with both kernel jobs being found unloaded at about that time.

Acceptance:
- Minting is atomic: the card write, the inbox move and the commit and push succeed together or are rolled back; a tick that starts with uncommitted record changes it did not make reports them and stops minting until they are resolved.
- Each tick checks the record checkout is clean and in sync with origin; a dirty or diverged checkout is a breach with the paths named, not silence.
- An item the kernel holds as minted but that is absent from origin is re-minted or reported within one tick.
- Test: a mint interrupted between write and commit leaves no untracked card after the next tick.
