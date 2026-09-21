# Control plane — the factory's dashboard, served from the record
type: epic

## Description
As an operator, I want one web page that shows the whole factory the way the console tables do —
and lets me act on it — so that I run the factory from a browser (and later a phone) instead of a
terminal, and the same page becomes the SaaS front end without a rewrite.

This is a starting point for discussion, not the final shape. The inspiration is a static
prototype built on 2026-09-21 over one product's real `index.json` (963 items):
https://claude.ai/artifact/PEEHGNrAjjqLC7AatgtTgP — a SaaS-style dashboard in the style of the
first product's ops console. What it has, and what the Epic keeps as its vocabulary:

- **Left sidebar** with a product switcher (one live product; the others listed as not yet adopted)
  and menu groups **Monitor** (Overview, Metrics, Sessions), **Plan** (Roadmap, Backlog, Board,
  Stories), **Build** (Tasks, Bugs), **Govern** (Rules, Decisions), **Operate** (Settings) — every
  entry with a live count.
- **Overview**: KPI tiles with state bars, the ranked roadmap, *Needs attention* (S1/S2 Bugs,
  blocked items), *Stale* (Active > 48 h), *Recently changed*.
- **Roadmap** = the tree (epic → feature → stories/tasks/bugs); **Backlog / Stories / Tasks / Bugs /
  Rules / Decisions** = sortable, filterable tables; **Board** = kanban by state or by Feature
  stage, one swimlane per Epic.
- **Top bar**: search, Epic and State filters, removed-items toggle, theme toggle; every row opens a
  **detail drawer** (state, stage, links, evidence, children, backlinks, file path); the URL hash
  carries view + filters so a link reproduces a view.
- **Extensible by design**: `PAGES.<id> = {title, group, count, render}` is the whole registry;
  Metrics and Sessions were registered placeholders naming the stream they will read.
- Design tokens: a dark ground, one accent, a sans + a mono face, badges ok/warn/danger/info/idle,
  radius 6/10/16 — generic, no product branding.

What the Epic adds beyond the prototype (to be decided Feature by Feature):

1. **Served, not embedded** — `asf serve --product <p>` (stdlib `http.server`) serves the page and
   `index.json`, the metrics streams and `sessions.jsonl` as JSON; the page never embeds data.
   Multi-product: the switcher reads `~/.ASF/products/`.
2. **Metrics and Sessions pages** fed from `metrics/ci|sessions|ticks|events` and the daily
   rollup: features on prod per week, USD per Feature, defects per release, tokens per session,
   CI red signatures — the scorecard, live.
3. **Operate** — the actions the console has, as buttons with the approval matrix behind them:
   answer the groom (one word per line), `pause`/`unpause` a product, `start`/`stop` the scheduler,
   approve a spec/plan, mark a prod check ✓, answer a `NEEDS OPERATOR` line. Every action is an
   `asf` library call (the SaaS seam: nothing only a shell can do) and is logged to
   `metrics/events` with who and when.
4. **Incidents first** — while an S1 is open, an INCIDENTS banner on every page (the S1 lane
   Feature's data).
5. **Phone layout** — the same page at phone width (16 px gutter, no horizontal scroll); the
   scorecard-on-a-phone use case the operator already has.
6. **Auth and exposure** — localhost only by default; a token for a LAN/phone; anything beyond is
   the SaaS Epic's problem, not this one's.
7. **Prototype → tests** — the static prototype's HTML becomes `asf/serve/static/`, its behaviour
   pinned by a headless test over a fixture record (golden counts per page, drawer contents, hash
   round-trip).

Open questions for the groom: vanilla JS (as the prototype) vs a framework; whether Board and
Roadmap are one page with two renderings; whether Settings edits `products/<p>.yaml` in place;
which actions need `human-now` vs `groom` in the approval matrix.

## Acceptance (Epic level)
- [ ] `asf serve --product <p>` shows every table the console shows, from the same `index.json`,
  with zero embedded data; counts match `asf status`.
- [ ] The operator answers a groom, pauses a product and acknowledges a NEEDS OPERATOR line from
  the page, each logged as an event.
- [ ] Works at phone width; localhost by default.

## History
- 2026-09-21 18:10 filed on the operator's request: "let the dashboard you did serve as inspiration
  for an epic regarding control pane for ASF. Its not the final version, but its a starting point
  to be discussed."
