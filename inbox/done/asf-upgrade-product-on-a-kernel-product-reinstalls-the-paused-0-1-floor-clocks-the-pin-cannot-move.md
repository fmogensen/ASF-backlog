→ F-0342

# asf upgrade --product on a kernel product reinstalls the paused 0.1 floor clocks; the pin cannot move

Bug: on a product run by the 0.2 kernel, `asf upgrade --product asf --to <sha>` cannot be used. Its plan (dry-run 2026-10-10 15:55Z) boots out and then RESUMES the 0.1 floor clocks (record-health-wave-prs-harvest, daily, wave) and runs `asf scheduler install --product asf`, which writes every clock still configured in the product yaml, putting the paused 0.1 floor back next to the kernel. So the product pin (state/asf/install.json, 0.1.265 = 14b963b from 10-08) cannot move, and hooks/sessions run an old asf (F-0339 needed `asf pre-push` from aa56423fd).
Fix: a kernel product's upgrade moves only the pin + hooks + the kernel clocks (asf kernel install), never 0.1 floor clocks; scheduler install skips floor clocks when the product has a kernel block.
Acceptance: a test where a product with a kernel block is upgraded writes only the kernel clocks; the pin moves; hooks call the new build.
