# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository layout

Single-file deliverable: `snake-on-fire.html` — HTML + CSS + vanilla JS in one file. No build tools, no package manager. **Dependencies in `<head>`:** Google Fonts `Press Start 2P`, and optionally two Firebase compat scripts from `gstatic.com` (Realtime Database leaderboard). **`index.html`** in this repo is kept identical to `snake-on-fire.html` for hosts that default to `index.html` (e.g. GitHub Pages).

**Smallest offline build:** set `USE_FIREBASE = false` at the top of `<script>` and remove the two Firebase `<script src=…gstatic…>` lines in `<head>`.

**Firebase rules in repo:** `database.rules.json`, `firebase.json`, `.firebaserc`. After `npm i -g firebase-tools` (or `npx firebase-tools`), run `firebase login` and `firebase deploy --only database` from the project root to publish rules. You can also paste the JSON from `database.rules.json` into the Firebase Console under Realtime Database → Rules.

## Running and testing

- **Run**: open `snake-on-fire.html` directly in a browser (`open snake-on-fire.html` on macOS). No server required; everything works from `file://`.
- **No test suite, no linter, no build step.** Verification is manual: open the file and play. After edits to game logic, sanity-check the state machine by walking INTRO → MENU → PCOUNT → NAME → COUNTDOWN → PLAYING → GAMEOVER → RESULTS → HIGHSCORE.
- **Persistent state** lives in `localStorage` under the key `snakeOnFire_hs` (top-10 highscore). Clear it via the browser devtools Application tab if you need to reset.
- **Global leaderboard** (optional): when `USE_FIREBASE` is true and the SDK loads, scores upload after a full round (`advanceAfterDeath` → `uploadScore`) via a **queue** flushed when RTDB connects; the **HIGHSCORE** and **RESULTS** screens show `globalTop10`. If writes fail (e.g. rules), `lastFbWriteError` is drawn on RESULTS and errors log to the console.

## Architecture

The game is a classic game-loop architecture driven by `requestAnimationFrame` and a state machine. Reading the file top-to-bottom roughly follows the dataflow:

1. **Canvas constants** (top of `<script>`): `W=768, H=528, CELL=24, COLS=32, ROWS=22`. Row 0 is reserved for HUD overlay — `spawnFood()` excludes it. Any "wall" semantics for the HUD bar must be enforced in `tickGame()`, not by canvas geometry.
2. **State machine**: `STATES` enum + global `state` variable. Each state has a `draw<State>()` renderer dispatched in `render()`, and input handling routed through `routeKey(e)`'s switch. Adding a state means touching all three places.
3. **Game tick vs. render tick are decoupled**:
   - `update(dt)` advances `tickTimer`; when it crosses `tickInterval` ms, `tickGame()` runs one logical step (consume one direction from `dirQueue`, move snake by one cell, check collisions, eat food).
   - `render()` draws every frame, interpolating snake position between `prevSnake` (snapshot taken at the start of `tickGame`) and current `snake` using `progress = tickTimer / tickInterval`. This is what makes movement smooth despite the discrete grid.
   - **Consequence**: never mutate `snake` outside `tickGame()` without also updating `prevSnake`, or interpolation will jump.
4. **Input is queued, not immediate**: `setDir(d)` appends to `dirQueue` (max length 2), compared against the last queued direction (not current `dir`) to prevent fast 90°+90° presses from effectively reversing into self-bite. `tickGame()` shifts one direction per tick.
5. **Fire-level escalation drives almost all visual variety**: `fireLevel()` returns a discrete tier from `foodEaten` count. The tier feeds into `snakeColors()`, particle spawn counts in `tickGame()`, the HUD label, and the hellfire overlay. To add a tier, extend `fireLevel()`'s thresholds and add branches in `snakeColors()`, `particleColor()`, `drawHUD()`, and any banner/overlay renderer.
6. **Food type dispatcher**: `spawnFood()` assigns `type` ∈ `{flame, ice, bonus}` based on `foodEaten + 1` (every 7th is ice, every 8th after an ice is bonus). Eating logic in `tickGame()` branches on `food.type`. Rendering dispatches in `drawFood()` → `drawFlame` / `drawIceCrystal` / `drawBonusFlame`. Ice has a sub-level (`iceLevel` 1–4) that evolves the visual.
7. **Pixel art is `ctx.fillRect`-based** with `image-rendering: pixelated` on the canvas CSS. Helper `drawBlockPx(px, py, fill, edge, shine)` is the workhorse for the 3-tone chunky look. The Feuertonne logo in `drawFireBarrel()` is hand-coded pixel coordinates — modify with care.
8. **Multi-player session flow**: `players = [{name, score}]`, `currentPlayer` indexes it. `advanceAfterDeath()` moves to the next player or to `RESULTS`. `RESULTS` writes into the localStorage highscore via `saveHs()` and queues global uploads via `uploadScore()`.

## Editing conventions

- **Edits go into the single file.** Do not split into separate JS/CSS files — the explicit goal is one-file portability.
- **German UI strings** appear in some screens (per user spec). Keep new UI text consistent with the existing language mix (gameplay labels in English caps, user-facing messages and the brand `FEUERTONNEN_PRODUCTIONS` as-is).
- **Press Start 2P font is mandatory for the retro look**; render text through `drawText()` which already handles letter-spacing and shadow.
- **When adding particles for a new fire tier**, extend both `particleColor(p)` (color computation per `kind`) and the call site in `tickGame()` that picks `count` and `kind` from `fireLevel()`.
- **When adding a state**, the three required touchpoints are: `STATES` enum, a `draw<State>()` function called from `render()`'s switch, and a case in `routeKey()`.

## User/product context

- The user is a non-coder; deliverables must work by double-clicking the file. Avoid suggestions that require terminals, dev servers, or installs.
- The aesthetic target is "90s Snake 2, modernized to retro pixel HD" — chunky pixels, CRT scanlines + vignette (already in CSS), Press Start 2P font, no anti-aliased curves.
- Self-bite is the **only** death condition. Wrap-around at left/right/bottom edges is intentional. The top edge (row 0) is the HUD bar and is treated as a wall.
