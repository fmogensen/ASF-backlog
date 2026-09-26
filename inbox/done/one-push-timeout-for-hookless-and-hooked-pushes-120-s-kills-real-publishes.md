→ F-0165

# One push timeout for hookless and hooked pushes: 120 s kills real publishes

e2a1803's single git.push_timeout_s (default 120 s) regressed real publishes: botseon's cloud/T-0091 and docs/brief-templates-backlog-block publish pushes ran the product pre-push hook >120 s on a loaded host (load ~50) and were killed ("push timed out … left as it was"), so session work stopped publishing. Hand fix: botseon conventions.git.push_timeout_s: 900. Code: split the budget — hookless pushes (archive/delete/tags) keep a short timeout (60–120 s); hooked content pushes get a long default (e.g. 900 s) or scale with the measured hook p90; log hook duration per publish. Test both defaults.
