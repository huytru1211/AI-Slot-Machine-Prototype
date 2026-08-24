# SlotMachineWebGL

## Single Source of Truth

**NEON_777_GDD.md is the only spec.** All Vite/Three.js references are dead.

## Deliverable

ONE `index.html` — inline CSS + JS, no frameworks, no build tools, no CDN.

## Engine Contract

- Pure game logic (CONFIG, strips, RNG, payline evaluator, simulate(n)) wrapped between:
  `// ===ENGINE-START===` and `// ===ENGINE-END===`
- No DOM, no localStorage, no audio inside the engine block
- `tools/sim.mjs` (dev-only, not a deliverable) extracts the engine block and runs `simulate(1_000_000)` in Node

## Gate (blocking)

§5 RTP 94–97% and hit frequency 28–34% must pass before building presentation layers.

## Dev Commands

```bash
node tools/sim.mjs   # run 1M-spin Monte Carlo, reports RTP + hit freq
```
