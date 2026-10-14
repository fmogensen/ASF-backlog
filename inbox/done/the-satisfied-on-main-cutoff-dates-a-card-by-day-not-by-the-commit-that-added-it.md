→ B-224761

# The satisfied-on-main cutoff dates a card by day, not by the commit that added it
parent: F-0360

Type: Bug

The still-needed gate and its re-judge date an item through asf/kernel/ports.py `_created`: `created:`, else the History "created" line (a date only), else `stage_since`, which asf/record/ingest.py:486 moves on any tracked edit. With a date only, asf/kernel/needed_probe.py `NeededProbe.cutoff` takes the end of that day, so a test method another item lands on the filing day after the card was minted never satisfies it, and the re-judge reopens such a Done. The record's own history holds the exact instant: the commit that added the card file, present for every live card and never moved by later edits.

## Acceptance
- [ ] The cutoff uses the card file's add-commit instant from the record history when readable, cached per item; the date rules stay the fallback.
- [ ] A test method landed on the filing day after the mint commit satisfies the item; one landed before it does not.
- [ ] An edit that moves stage_since does not move the cutoff.
