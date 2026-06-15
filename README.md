# sakura

Cherry blossom petals drift across the app. They sway side to side, tumble in 3D as they
fall, and drift sideways in the wind. Petals that leave the screen are recycled from the top.

## Commands

- `toggle` — turn on/off
- `density n` — number of petals (5–150, default 32)
- `fall speed` — fall speed multiplier (0.3–3)
- `wind strength` — wind strength (-2–2, negative = leftward)

## Performance Contract

This is a secondary effect; the goal is zero impact on app performance:

- All petals are drawn on a **single `<canvas>`** — one composite layer (replaces the original
  version that used N DOM petals = N layers + expensive 3D recompositing). 3D tumble is faked
  with a 2D `scaleX(cos)` flip (cheap on the GPU).
- DPR capped at 1.5 (petals are inherently soft — Retina resolution is unnecessary), reducing
  canvas pixel and clear costs.
- Frame rate capped at 60 fps; no layout reads per frame.
- **Fully paused when unfocused, hidden, or not visible** — rAF is cancelled entirely, not
  throttled (zero cost). It stops when not being watched.
- Petals are recycled when they leave the screen — no object creation or destruction.

Inspired by: [jhammann/sakura](https://jhammann.github.io/sakura/), [ludwan/cherryBranch](https://github.com/ludwan/cherryBranch).
