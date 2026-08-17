# Tests

## `gates.js` — end-to-end gate suite (19 checks)

Loads the real `index.html` in jsdom, then independently reimplements the documented
board-generation spec and uses it to **solve the daily board optimally**.

The strongest assertion in here: after clicking each tile exactly its minimum-rotation
count, **moves must equal par exactly**. That simultaneously proves the generator, the
rotation maths, the flood-fill, and the par calculation all agree — and that par is
genuinely achievable rather than a number we print.

Covers: grid structure (no stray FX nodes in the grid container), spec agreement,
archive bounds, endless grid sizing, optimal solve, win flow, streak, share-card
spoiler safety, persistence, and runtime errors.

```bash
npm install jsdom
node test/gates.js
```

Exit code is non-zero if any gate fails.

### Bugs this suite has already caught
1. **Particles appended to the grid container.** `#board` is `display:grid`, so every
   particle became a grid item and shifted the board. Fixed with a sibling `#fx` overlay.
2. **Archive served a board dated before the game existed.** On day 1 it picked
   "yesterday", producing a board older than puzzle No.1. Archive is now bounded to
   `[No.1, yesterday]` and the tab disables itself until there is real history.
