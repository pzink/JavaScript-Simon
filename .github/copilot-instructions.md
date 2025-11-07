## Purpose

Short guidance to help an AI coding agent be productive in this repository: a very small, single-page JavaScript “Simon” clone in `simon.html`.

## Repo overview (big picture)

- Single-file web app: `simon.html` contains HTML, CSS links, and all JavaScript. There is no build system, package manager, or server-side code.
- UI is a 2×2 HTML table (`id="table1"`) whose cells represent the four Simon pads. Interaction is wired inline via `onMouseDown` attributes that call `yourTurn(n)`.
- Core gameplay functions: `firstSquare()`, `playMemory()`, `nextSquare()`, `nextRound()`, `yourTurn(cellNumber)`, `clearSquares()`.
- Global state lives in top-level JS variables: `cellOrder` (Array), `i`, `j`, `k`, and `speed`. The random generator is a custom Central Randomizer (`rnd()` / `rand()` functions).

## Key files to inspect

- `simon.html` — everything. Look here for DOM structure (`table1`, cell `id`s `cell1..cell4`), game logic, and inline styles.
- `README.md` — one-line project description.

## How to run / developer workflow

- No build step. Open `simon.html` in a browser to run the game. For a local HTTP server (recommended for console/asset access):

```bash
# from repo root
python3 -m http.server 8000
# then open http://localhost:8000/simon.html
```

- Debug with browser DevTools: set breakpoints in the inline script, or add `console.log` in `playMemory()` / `yourTurn()`.

## Project-specific patterns & gotchas

- Inline event wiring: cells use `onMouseDown="yourTurn(n)"` rather than `addEventListener`. Search for `onMouseDown` to find all user-facing entry points.
- DOM access style is mixed and sometimes fragile: the code often uses `document.forms[0].elements['startButton']` but the Start button is defined with `id="startButton"` (no `name` attribute). Use `document.getElementById('startButton')` to be explicit when editing.
- Timers use `setTimeout("playMemory()", speed)` — the string form evaluates globally; prefer passing function references when refactoring (e.g., `setTimeout(playMemory, speed)`).
- Colors and mapping are defined in `switch` statements inside `playMemory()` and `yourTurn()` — change RGB values there to update pad colors.
- Game speed is controlled by the global `speed` variable (initialized near top) and adjusted in `nextRound()` with `speed = speed-(100/j)`. Be careful with division by zero when refactoring.

## Editing examples (concrete snippets to modify)

- Change pad colors: edit the `onColor` assignments in the `switch (onCell)` blocks (both in `playMemory()` and `yourTurn()`).
- Adjust starting speed: edit the top-level `speed` variable.
- Replace inline handlers with listeners: remove `onMouseDown` attributes and add `element.addEventListener('mousedown', () => yourTurn(n))` in a small DOM-ready snippet.

## Integration & dependencies

- No external libraries or services. Randomness comes from the included Central Randomizer implementation.

## Testing / CI notes

- There are no automated tests or CI configuration in this repository. Keep edits minimal and verify manually by running the served page in a browser.

## Helpful search terms

- `table1`, `cellOrder`, `firstSquare`, `playMemory`, `yourTurn`, `Central Randomizer`, `setTimeout`.

## If you need to modernize

- Any modernization (module bundlers, unit tests, or separating JS into files) is outside the current structure. If requested, propose a small migration plan (move inline script to `js/` folder, add `index.html` that loads it, add a tiny test harness), but do not perform large refactors without user approval.

---
If any of this is unclear or you'd like more detail in a particular area (testing, refactor suggestions, or a live dev workflow), tell me which part to expand and I'll update this file.
