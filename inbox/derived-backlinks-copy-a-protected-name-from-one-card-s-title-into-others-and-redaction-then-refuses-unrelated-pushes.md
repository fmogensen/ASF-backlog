# Derived backlinks copy a protected name from one card's title into others, and redaction then refuses unrelated pushes

ASF copies card titles into derived text. `asf index` writes Backlinks and Children lines that quote other cards' titles. So a single title containing a protected name (a worker account name, an operator name: anything the redaction gate forbids) spreads into cards the author never touched, and the pre-push redaction gate then refuses unrelated pushes. Seen on the first customer install: migrated titles ending in an account name spread into a derived Backlinks line and blocked a push.

Want:
1. `asf check` flags a protected name in any typed field (title first) at the card that holds it, with the line. The groom shows it as a fix line.
2. Derived text (Backlinks, Children, digests, tables) passes titles through the redaction filter before writing: the name is replaced with a neutral token, never copied.
3. Intake of inbox cards applies the same filter to the title.

Tests: a protected name in a title → a check finding; derived Backlinks never contain it; intake strips it.
