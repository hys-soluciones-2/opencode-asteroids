# AGENTS.md

## Repo shape
- Static, dependency-free HTML5 Canvas Asteroids clone. `index.html` loads `game.js` as a classic `<script>` (not a module); all state, classes, and the loop live in `game.js`. No package manifest, bundler, or module boundaries — don't add `import`/`export` without changing how it's loaded.
- Canvas size `800x600` is duplicated in the `<canvas>` attributes in `index.html` and the `W`/`H` constants in `game.js`. Change both together.

## Run / verify
- There is no build, test, lint, typecheck, or CI config. Don't invent commands.
- Verify by opening `index.html` in a browser, or `npx serve .` then `http://localhost:3000` (per README). Manual play is the only check available.

## Execution flow / gotchas
- `game.js` ends with `initGame(); requestAnimationFrame(loop)`. Each frame runs `update(dt)` then `draw()`; `dt` is clamped to 0.05 s.
- Game state is `'playing' | 'dead' | 'gameover'`. Space restarts from `'gameover'`; `'dead'` waits `deadTimer` seconds, then calls `ship.reset()`.
- Input uses `e.code` (`'Space'`, `'ArrowLeft'`, …), not `e.key`. `justPressed` is set only on the key-down transition and `pressed(code)` consumes the flag, so a second `pressed()` for the same code in one frame returns false. Space and arrow keys are `preventDefault`ed.
- `RADII`, `SPEEDS`, `POINTS` are 1-indexed by asteroid `size` (1 = small/100 pts, 3 = large/20 pts); index 0 is an unused placeholder. `split()` halves the size and returns nothing for size 1.
- User-facing strings and code comments are in Spanish (`NIVEL`, `PUNTAJE`, `ESPACIO PARA REINICIAR`). Keep new UI text in Spanish.

## Known doc drift
- `README.md` advertises power-ups and a "estrella fugaz" asteroid type that do not exist in `game.js`. Trust the code.
