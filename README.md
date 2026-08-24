# NEON 777 🎰 — Classic Vegas Slot Machine (AI-Built Prototype)

> **Part 2 deliverable** for the Laser Focus *AI Game Designer* assignment: a playable slot machine built with AI, plus an explanation of which outcomes are wins and why.

**▶ Play now:** [https://slot-machine-prototype.netlify.app]

> **Reviewer tip:** press `D` (desktop) or tap the **NEON 777 logo 5 times** (mobile) to open the debug panel — force the jackpot, free spins, wilds or a near-miss instantly instead of waiting on the real odds. Forced spins are watermarked **◆ FORCED ◆** and still run through the real engine.

*Mobile-first portrait (~390 px), works with touch and mouse. Tap once to enable sound (browser autoplay policy).*

---

## At a glance

| | |
|---|---|
| Format | 3 reels × 3 rows, **5 fixed paylines** |
| Math core | 32-stop **weighted reel strips** — real casino architecture, nothing scripted |
| RTP | **~95.66%** (design gate: 94–97%) |
| Hit frequency | **~32.29%** (design gate: 28–34%) |
| House edge | **~4.34%** |
| Verification | 1,000,000-spin Monte Carlo (Node) + live 200,000-spin sim inside the game |
| Features | Wild, Scatter, Free Spins ×2 with retrigger, progressive jackpot, anticipation system, 3 celebration tiers |
| Tech | **One `index.html`** — inline CSS/JS, zero frameworks, zero CDN, ~1 MB including embedded CC0 audio |
| Money | Play credits only. No real money, no IAP. |

---

## How to play (30 seconds)

1. You start with **1,000 credits**.
2. Set **LINE BET** with − / + (1, 2, 5 or 10). All 5 lines are always active → **total bet = 5 / 10 / 25 / 50**.
3. Hit **SPIN**. Reels stop left → right; winning lines light up one by one; the win rolls up into your balance.
4. Out of credits? **CASHIER** refills you to 1,000.
5. **PAY** opens the full paytable, payline diagrams, live odds, and design notes.
6. In a hurry? Open the **debug panel** (`D` key, or 5 quick taps on the logo) and force any outcome — jackpot, free spins, near-miss — in seconds.

---

## What counts as a win — and why

### 1. The five paylines

Each spin shows a 3×3 window of symbols. A line win needs **3 matching symbols left-to-right** along one of these paths (cherry is the single exception — see the paytable):

| # | Line | Path (Reel 1 → Reel 2 → Reel 3) |
|---|---|---|
| 1 | Middle | mid – mid – mid |
| 2 | Top | top – top – top |
| 3 | Bottom | bottom – bottom – bottom |
| 4 | Diagonal ↘ | top – mid – bottom |
| 5 | Diagonal ↗ | bottom – mid – top |

Every line is evaluated independently. Several lines can win on the same spin and their pays **add up**. If one line could be read as more than one combo, it pays the **highest** one only.

### 2. Paytable

Line wins pay in multiples of your **line bet**; the scatter pays on your **total bet**.

| Combo | Pays | Why it's a win |
|---|---|---|
| 7️⃣ 7️⃣ 7️⃣ | **75× line bet** — at max line bet (10) it pays the **PROGRESSIVE JACKPOT** instead | Rarest 3-of-a-kind on the strips → top prize |
| 💎 💎 💎 | **38×** | All-wild line, second rarest |
| BAR BAR BAR | **15×** | High symbol |
| 🔔 🔔 🔔 | **8×** | Mid symbol |
| 🍋 🍋 🍋 | **5×** | Common symbol, small pay |
| 🍒 🍒 🍒 | **3×** | Common symbol, small pay |
| 🍒 🍒 on reels 1–2 | **1×** | Frequent micro-win that keeps momentum. Requires **both** reels 1 and 2 to show cherry (or wild-as-cherry); if reel 3 completes it, the line upgrades to cherry ×3 instead — never double-counted |
| ⭐ ⭐ ⭐ **anywhere** in the window | **2× total bet** | Scatter — plus **8 Free Spins, all wins ×2** (base game). During Free Spins, 3 scatters **retrigger +5 spins** (max once) |

**Special symbols**

- 💎 **WILD** substitutes for any symbol **except** ⭐. Example: `7 💎 7` pays as `7 7 7`.
- ⭐ **SCATTER** counts in any of the 9 positions, needs no payline, and cannot be substituted by wilds.

### 3. Why these outcomes and these numbers

**Pays follow rarity.** Each reel is a fixed 32-stop strip; the RNG (`crypto.getRandomValues`, `Math.random` fallback) picks one stop per reel, and the visible window is the three symbols around that stop. Every probability in the game is therefore fully determined by the strip composition:

| Symbol | Reel 1 | Reel 2 | Reel 3 | Design intent |
|---|---|---|---|---|
| 7️⃣ | 3 | 3 | **2** | Deliberately scarcer on reel 3 |
| 💎 Wild | 1 | 1 | 2 | Rare helper |
| BAR | 8 | 8 | 5 | Bread-and-butter high pay |
| 🔔 | 7 | 7 | 6 | Mid |
| 🍋 | 7 | 7 | 8 | Filler low |
| 🍒 | 3 | 3 | 6 | Micro-win fuel |
| ⭐ | 3 | 3 | 3 | Bonus trigger |

Four consequences of that table:

- **Pay is the inverse of probability.** A given line hits `7-7-7` at roughly `3/32 × 3/32 × 2/32` ≈ **1 in ~1,800** (before wild substitution) — that rarity is what justifies 75× and the jackpot. Lemons land constantly, so they pay 5×. Weights and pays were tuned together until the whole machine sits at ~95.66% RTP.
- **Near-misses are real math, not scripts.** Because 7 has only 2 stops on reel 3, `7-7-x` appears far more often than `7-7-7`. The dramatic slow-down on reel 3 (anticipation) *reacts* to an outcome that was already decided before the animation started — it never changes the result.
- **Medium volatility on purpose.** Frequent cherry/lemon micro-wins keep the session moving, while 7 / 💎 / jackpot live in the tail of the distribution and create the big moments.
- **The house edge is honest.** RTP ~95.66% means the machine keeps ~4.3% of everything bet over time, like a real slot. Wins smaller than your bet are displayed plainly and never celebrated; losses are completely silent — the game never lies about how you're doing.

**Don't take my word for it — verify:**

- In-game: **PAY → ODDS** runs a live 200,000-spin simulation in your browser and prints RTP / hit frequency next to the exact strip counts.
- Dev: `node tools/sim.mjs` extracts the *shipped* engine (the pure-logic block between `// ===ENGINE-START===` and `// ===ENGINE-END===`) and runs a 1,000,000-spin Monte Carlo — the numbers above test the real engine, not a copy.
- Debug panel (`D` key, or 5 quick taps on the logo): force 7-7-7, wilds, free spins, near-misses or losses. Forced spins are watermarked **◆ FORCED ◆** and still go through the real evaluator.

---

## Feature tour

**Free Spins** — 3 ⭐ anywhere → 8 free spins with **all wins ×2**. The cabinet re-skins to a gold theme, reels spin 35% faster (1080 vs 800 px/s), star particles rain, and a music loop kicks in. 3 ⭐ during the bonus retriggers **+5 spins** (max once). The bonus ends with a 3-step recap: freeze-frame → total roll-up → full celebration tier for the combined win.

**Progressive Jackpot** — the gold ticker above the reels seeds at 5,000 and grows by 1% of every bet (plus a small ambient drip for casino-floor feel). Hitting **7-7-7 at max line bet** pays the ticker and resets it; at lower bets, 7-7-7 pays the normal 75×. The jackpot persists across reloads.

**Anticipation** — when reels 1–2 land a promising pair, reel 3 decelerates dramatically with a heartbeat sound and glowing frame. "Super" anticipation (gold strobe) only fires when a big win actually landed — the strongest tease is never a lie.

**Celebrations** — BIG (≥10× bet), MEGA (≥25×), EPIC / JACKPOT (≥60× or 7-7-7): banner slam, physics-based coin shower (30 / 80 / 150 coins), cabinet shake, marquee strobe, and tiered haptics.

---

## How I used AI

This project was built almost entirely with **Claude Code in the terminal** — but spec-first, not prompt-and-pray. The actual workflow:

**1. Rules before prompts.** `CLAUDE.md` is a standing rules file Claude reads before acting on any prompt: one `index.html`, no frameworks, no CDN, and a pure-logic engine isolated between `// ===ENGINE-START===` / `// ===ENGINE-END===` markers (no DOM, no audio, no storage inside). This turned the AI from a free-styling generator into a constrained contractor.

**2. AI-drafted spec, human-approved.** I first researched how real slot machines work (weighted reel strips, paylines, RTP, house edge), then had Claude draft the full `NEON_777_GDD.md`. I reviewed it, corrected everything that didn't match my design intent, and locked it as the single source of truth. From then on, every prompt pointed back at the spec instead of re-describing the game.

**3. A blocking math gate before any juice.** The rule in `CLAUDE.md`: RTP must land in **94–97%** and hit frequency in **28–34%** *before* the AI was allowed to build presentation layers. We iterated on strip weights and the paytable against `simulate(1_000_000)` until the sim reported **95.66% / 32.29%**.

**4. Build → play → note → fix loop.** Claude built the game to a local `index.html` I opened directly in the browser. I playtested it like a player, wrote down everything that felt wrong (timing, game feel, unclear win presentation), and fed those notes back as concrete change requests — one issue per prompt, never "make it better."

**5. Console-driven debugging.** Whenever something broke, I opened DevTools (Ctrl+Shift+I), copied the exact console error, and pasted it to Claude. A real stack trace gets fixed in one iteration; "it's broken" doesn't.

**6. The simulator as referee.** Because the engine block is pure, `tools/sim.mjs` extracts the *exact shipped code* and Monte-Carlos it in Node — so Claude could never "pass" the RTP gate by testing a hand-copied model, and the published numbers come from the real engine. When Claude's tuning missed the gate, the sim caught it, not me.

**7. Docs never drift.** After every accepted change, Claude was required to update the GDD with the new feature, numbers, or behavior — so the spec in this repo describes the *as-built* game, not an early draft.

**8. A second AI as independent reviewer.** Separately from Claude Code, I set up a **Claude Project** (web) loaded with the GDD, `CLAUDE.md`, the assignment brief, and this README — with strict instructions to answer only from those sources and to cite exactly where in the docs every claim comes from. After each new build or doc update, this reviewer audited the whole package for spec drift, internal contradictions, and unfinished sections. It caught real issues the builder session missed (stale status flags in the GDD, leftover placeholder blocks) before submission — a cheap cross-check that keeps one AI from grading its own homework.

**Role split.** Design decisions were mine: the paytable feel, medium volatility target, celebration thresholds, and the honesty rules ("losses are silent, sub-bet wins are never celebrated, super-anticipation never fires on a loss"). Claude did the heavy lifting: code generation and refactoring, strip tuning against the gate, Web Audio synth graphs, and base64-embedding the CC0 audio.

---

## Tech notes

- **One file.** `index.html`, inline CSS + JS. DOM/CSS renders the cabinet and reels; a single fullscreen `<canvas>` handles coin and star particles.
- **Audio:** pure Web Audio API — a synth layer (oscillators, filtered noise, FM "metal" strikes) layered with embedded base64 OGG buffers. Initialised only after the first tap; global mute persists.
- **Persistence:** `localStorage` for credits, jackpot, mute, and best win.
- **Accessibility & polish:** `prefers-reduced-motion` collapses all animation; tap targets ≥ 44 px; haptics via `navigator.vibrate()` degrade silently when unsupported.

## Audio credits (all CC0)

Kenney *Casino Audio* pack (chip / reel-stop sounds), Kenney *Music Jingles* ("Alpha Dance" — free-spins loop), Zane Little *Win Jingle* (intro brass fanfare), and *winfretless* (big-win jingle) — via opengameart.org and gamesounds.xyz.

## Design stance

This machine uses real casino architecture and a real house edge. The excitement lives entirely in the presentation layer — anticipation slow-downs, line-by-line reveals, accelerating roll-ups, tiered celebrations — while the odds stay fixed, inspectable, and simulated in the open. It plays with fake credits only: no real money, no purchases.
