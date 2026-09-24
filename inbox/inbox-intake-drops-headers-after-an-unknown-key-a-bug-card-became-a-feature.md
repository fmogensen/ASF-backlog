# Inbox intake drops headers after an unknown key; a Bug card became a Feature

signature: "inbox intake stops reading headers at the first unknown key, and silently drops the rest (signature: included)"
severity: S2

## Description
- `groom/inbox.py` reads a card's header lines with `INBOX_KV_RE` (type|parent|signature|severity|writes|stories), and the first line that does not match ends the header block.
- A card whose headers were `parent:`, `after:`, `signature:`, `severity:` lost everything after `after:`. The signature was never read, the shape fell to `default`, and a defect became a decided Feature. Under a feature hold, that means it would never launch.
- The same thing happened this morning with `type: bug` placed after the header. Twice in one day, the cause was a header line intake does not know about, dropped without a word.

## Expected
- Intake either accepts `after:` (it is a valid item field) or refuses the card, naming the unknown header line. It never drops the header lines that follow one it doesn't know.
- A test: a card with an unknown header line before `signature:` is refused or shaped as a Bug, never as a Feature.
