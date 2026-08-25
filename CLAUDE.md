# NEON 777 — single-file slot machine

## Single Source of Truth

**NEON_777_GDD.md is the only spec.** All Vite/Three.js references are dead.

**Sync rule:** any change to engine math, features, or presentation MUST update
NEON_777_GDD.md in the same commit. If code and GDD disagree, stop and reconcile
before continuing.

## Deliverables — assignment Part 2 requires BOTH

1. **ONE `index.html`** — inline CSS + JS, no frameworks, no build tools, no CDN.
2. **README.md** — plain-language explanation of which outcomes are wins and why:
   paytable summary, wild/scatter rules, and RTP + hit frequency verified by the
   1M-spin sim. This is a required assignment deliverable, not optional docs.

## Engine Contract

- Pure game logic (CONFIG, strips, RNG, payline evaluator, simulate(n)) wrapped between:
  `// ===ENGINE-START===` and `// ===ENGINE-END===`
- No DOM, no localStorage, no audio inside the engine block
- `tools/sim.mjs` (dev-only, not a deliverable) extracts the engine block and runs
  `simulate(1_000_000)` in Node

## Gate (blocking)

GDD §6: RTP 94–97% and hit frequency 28–34% must pass before building
presentation layers.

## Definition of Done

Every item in the GDD §21 acceptance checklist is ✅ — including README.md.

## Dev Commands

```bash
node tools/sim.mjs   # run 1M-spin Monte Carlo, reports RTP + hit freq
```
