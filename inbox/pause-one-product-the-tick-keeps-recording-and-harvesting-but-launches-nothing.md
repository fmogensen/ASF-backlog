# Pause one product: the tick keeps recording and harvesting but launches nothing

## What is wanted
An operator can pause one product: `asf pause --product <p>` / `asf unpause --product <p>` (and the
matching plugin skills). While paused, the tick for that product keeps recording, ingesting,
harvesting finished branches and sweeping, but launches no new session and lands nothing new; every
table shows `PAUSED` on the product's row. Other products keep running.

## Evidence (reported by the first customer install)
The operator asked for pause/unpause per product after a console reset: today the only lever is
unloading the whole scheduler, which stops every product, or editing capacity in the product yaml.
The control-plane Epic lists pause/unpause as a dashboard action, but no CLI primitive exists for it to call
(`asf --help` has no pause).

## Fix direction
- A `paused` marker in the product's state dir (`asf.env.state_dir(product)`), written only by the
  command; the wave and harvest's landing path read it; `asf status`, `asf next` and `asf doctor`
  report it with the time it was set.
- A session-start hook in the plugin prints `asf status` for the product that owns the working
  directory, so a fresh console sees paused products at once.

## Test
Two fixture products, one paused: an hour of ticks launches nothing for the paused one and keeps
launching for the other; its finished branches are still recorded; unpause restores launching on
the next tick.

## Question
Which Epic is this under? No open Epic shares a title word with it.
