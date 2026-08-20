# AGENTS.md

## Stack
Static triple, no toolchain. `index.html` (canvas 800x600, mounts `game.js`) + `game.js` (entire game, ES6 classes) + `favicon.svg`. No `package.json`, no tests, no lint, no formatter, no CI. All logic lives in `game.js`; constants like `W`/`H`/`RADII`/`SPEEDS`/`POINTS` are module-scope, per-entity tunables (e.g. `ROT`, `THRUST`, `DRAG` in `Ship.update`) live inside the class method.

## Run
Open `index.html` directly, or:
```bash
npx serve .
```
Then visit `http://localhost:3000`. There is no build step and no automated verification — manual play is the only check.

## Input
Indexed by `e.code` (`'Space'`, `'ArrowUp'`, `'ArrowLeft'`, `'ArrowRight'`). Arrow keys and Space call `preventDefault()` to suppress page scroll, so add new reserved keys to that list too. `pressed(code)` is **consume-on-read**: it returns and clears `justPressed[code]`. Call it exactly once per frame from inside `update()` (currently in the main firing check and the `'gameover'` branch); duplicate calls or moving it elsewhere will break single-shot behavior.

## World model
Toroidal wrap via `wrap(v, max)` for x/y movement (ship, bullets, asteroids, particles). Collision math is naive `dist()` and **does not** account for wrap-around — preserve that convention; do not "fix" it without checking, since the gameplay is tuned around it.

## State machine
`state` is `'playing' | 'dead' | 'gameover'`. Lives/level/score are separate module vars; `initGame()` resets everything for a fresh run, `nextLevel()` resets ship + bullets + particles and spawns `3 + level` asteroids, `Ship.reset()` only resets the ship. When adding respawn/death logic, edit the matching function rather than duplicating reset code.

## Repo-specific conventions
- HUD text is Spanish (`SCORE`, `NIVEL`, `GAME OVER`, `PUNTAJE`). Don't translate without asking.
- HUD ships (`drawLifeIcon`) reuse the player's triangle silhouette rotated -90°.
- `lastTime === null` produces `dt = 0` on the first frame to avoid a startup jump — keep that pattern if adding new loops.
- Game-over overlay restarts on `pressed('Space')`; any new "press to start" affordance should reuse `pressed()`, not raw `keys`.

## README drift
`README.md` advertises features that do not exist in `game.js` (power-ups, "estrella fugaz" asteroid type). Do not implement them speculatively to match the README — flag the gap and ask before adding.
