→ F-0217

# Loop guard parks on the head read before the same tick's publish

Seen on F-0003 2026-09-27: the loop guard parked the item on the head read before the same tick's factory publish moved it (published b7331004f in that tick); unparked by hand. A factory publish that lands the session's commits in the tick must count as progress, or the park decision must read the head after publish.
