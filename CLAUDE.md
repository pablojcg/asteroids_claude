# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the game

Open `index.html` directly in a browser, or serve it locally:

```bash
npx serve .
```

Then visit `http://localhost:3000`. There is no build step, no bundler, and no dependencies.

## Architecture

Everything lives in a single file: `game.js`. The canvas is 800×600 (`W`/`H` constants).

**Classes:** `Ship`, `Asteroid`, `Bullet`, `Particle` — each has `update(dt)` and `draw()` methods, plus a `dead` boolean used to flag removal at the end of each frame.

**Game loop:** `initGame()` seeds global state, then `requestAnimationFrame(loop)` drives `update(dt)` → `draw()` every frame. `dt` is capped at 50 ms to prevent tunneling on tab-blur.

**Input:** `keys` holds currently-held keys; `justPressed` is consumed once per frame via `pressed(code)` for single-fire actions (shoot, restart).

**Toroidal space:** all positions wrap with `wrap(v, max)` so objects exit one edge and re-enter the opposite side.

**Asteroid splitting:** size goes 3 → 2 → 1; `Asteroid.split()` returns two smaller instances. `RADII`, `SPEEDS`, and `POINTS` arrays are indexed by size (index 0 unused).

**Game states:** `'playing'` | `'dead'` (respawn timer) | `'gameover'`. The `update()` function dispatches on `state` before running normal physics.
