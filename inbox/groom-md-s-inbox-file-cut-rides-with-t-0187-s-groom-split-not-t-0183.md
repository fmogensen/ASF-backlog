# groom.md's inbox:<file> cut rides with T-0187's groom split, not T-0183
parent: F-0093

The operator console dropped T-0183/touch_amendable_set (2026-09-27): T-0183 cuts the
`inbox:<file>` bullet from asf/briefs/templates/groom.md, but until T-0187 splits the groom day
(`groom_rows`, the `groom-clerk` row) every `inbox:` line still reaches the heavy groom row —
today's groom/2026-09-27.md carries 6. Landing the cut alone strips the only instruction the
groom has for them, while `no` in its general grammar closes an inbox card unminted
(asf/groom/inbox.py _CLOSE_RE). The cut is right only together with the split.

Wanted: the groom.md cut (and its golden) lands with T-0187, by the operator console, since
templates are the amendable set (F-0024); T-0183's branch lands without that hunk.
