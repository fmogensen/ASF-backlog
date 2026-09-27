# Lane branches collide on sequential numbers (migrations, bands); ASF should reserve them

Parallel lane branches pick the same sequential numbers (migrations, decision bands). 2026-09-27 on botseon: cloud/T-0383 used migration 0292 after 0292_bot_locale.sql had landed on main (main's bands check then went red, holding prod; botseon's #872 changes the check so the landed file keeps its claim). Also in flight: 0290 on cloud/T-0386 and cloud/direct-F-0112; 0297/0298 unbanded on cloud/direct-F-0113.

Ask: a factory-level number reservation. When a session needs the next number of a declared sequence (product config: e.g. `conventions.sequences: {migrations: "db/migrations/NNNN_*.sql", bands: docs/decisions/bands.md}`), it asks ASF (`asf reserve <sequence>`), which returns max(main, every open lane branch, reservations)+1 and records the claim; the rebase-before-publish step re-checks and renumbers a collision against main automatically, instead of a correct round.
