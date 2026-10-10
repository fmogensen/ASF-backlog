# Intake cannot mint an Epic

type: bug
severity: S2
parent: E-0003
signature: intake cannot mint an Epic: asf new epic refuses, intake verdict kind is feature|bug only

An inbox note declaring an Epic (ASF 0.4 candidate list) was minted F-0345, a Feature under E-0001 (first of several Epics tied on title words), and a spec session launched. Fix: `type: epic` on a note mints an Epic by code (no parent, body kept, declared priority kept), intake-decide verdict kind may be epic, an Epic never takes a parent, a tie between Epics is no parent guess, Resolved/Closed Epics are never guessed.
