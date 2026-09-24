# A removed or moved Feature still shows a derived stage (plan-approved) in index and tables

A removed or moved Feature (`removed:` / `moved_to:`) correctly gets no Tasks and no rows, but its derived `stage` still reads e.g. plan-approved in the index and the tables. Expected: no derived stage, shown as "removed" / "moved → <target>". Reported by the first customer install. Test: a removed Feature with a plan on main → stage removed.
