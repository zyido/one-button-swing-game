# One-button swing mechanic — canvas demo

The whole game is one input: **hold to grab the next hook, release to fly.**
No jump key, no run key. This repo is a small canvas demo of that mechanic plus
notes on why it is harder than it looks.

> Fan-made demo of the mechanic. Not the original game and not affiliated with
> its developer (Madbox). No original assets are included.

## What's here

| Path | What it is |
| --- | --- |
| `index.html` | The whole demo: canvas, pendulum physics, input and drawing |
| `README.md` | How to run it, what it implements, and the notes |

## Run it

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

Single HTML file, no build step, no dependencies. Opening `index.html` directly
also works — there is no fetch and no module loading, so nothing needs a server.

## What the demo implements

- Hook detection within grab radius on hold
- Pendulum swing while held (angle + angular velocity)
- Release inherits tangential velocity
- Landing check against the platform at the level end

That is the minimum that feels like the real thing. Anything less and release
timing stops mattering.

The physics runs in fixed 1/120 s steps with semi-implicit Euler, so a slow frame
does not change the trajectory. The pendulum conserves energy to within the
integration error — the swing does not quietly gain speed while you hold.

## The five mistakes that end most runs

1. **Releasing too early.** The instinct is to let go as soon as you touch the
   hook. Momentum peaks later in the arc — release near the bottom-forward part
   of the swing.
2. **Holding too long.** Holding past the forward peak swings you backward. The
   hook is not a checkpoint, it is a slingshot.
3. **Chaining hooks without reading the gap.** Two hooks close together are not
   a free combo; the second one is where overshoot happens.
4. **Landing at full speed.** You only need to touch the platform, not sprint
   across it. Over-travel causes most of the "I was right there" fails.
5. **Fighting the rhythm on retries.** After a fail, players tend to release
   earlier and earlier. Reset to the same timing you used on the successful
   attempt instead of compensating.

## Design note

Difficulty here comes from **hook spacing**, not from adding inputs. Tightening
the gap between two hooks raises difficulty without breaking the one-button
promise. The moment you add a second button, this stops being the same genre.

## Links

- Play Stickman Hook — https://stickman-hook.org/
- Stickman games — https://stickman-hook.org/category/stickman
- 2 player games — https://stickman-hook.org/category/2-player-games

## License

MIT for the demo code. The name and character belong to their owners; this repo
ships no assets from the original game.
