# RELAY — a daily signal puzzle

**Play: https://westonhughes07-ux.github.io/relay/**

Rotate the circuit until every node carries signal. One board a day, identical for everyone,
scored like golf against a provable par.

New project, built 2026. Free, no signup, no tracking. Everything runs in the browser.

---

## What it is

A daily circuit-routing puzzle. The signal starts at a bright core in the middle of the grid and
flows through connected wire. Rotate tiles until every node is live.

**The differentiator is the scoring.** Most daily puzzles count guesses. RELAY counts rotations
against **par** — the provably fewest turns the board can be solved in, computed at generation
time. Matching par is a real achievement, not a lucky streak, and it gives skilled players
something to chase after they can already solve every board.

## Modes

| Mode | Grid | Notes |
|---|---|---|
| **Daily** | 7×7 | Seeded from the UTC date, so every player worldwide gets the same board. |
| **Endless** | 5×5 → 9×9 | Unlimited boards; difficulty scales as you solve more. |
| **Archive** | 7×7 | Replay any past daily. |

## Features

- Streak and max streak, stored locally
- Telemetry panel with a distribution histogram bucketed by strokes over par
- Spoiler-free share card (puzzle number, moves, par, time, efficiency bar) — never reveals the solution
- Countdown to the next daily
- Undo (`Z`), Reset, and 3 hints per board (`H`) — each locks one tile correct for +5 moves
- Full keyboard play with visible focus rings

## How the boards are generated

Every board is **solvable by construction**, never by luck:

1. Carve a random **spanning tree** over the whole grid with a randomised DFS from the centre.
   That tree is the solved state, so every cell has at least one connection and all cells are
   reachable from the source.
2. **Scramble** by rotating each tile 0–3 times at random.
3. **Par** = the sum over all tiles of the fewest clockwise turns back to the solved orientation.
   Symmetric pieces (straight wire) match at more than one rotation, so the minimum is taken.

Randomness comes from a seeded `mulberry32` PRNG keyed by an FNV-1a hash of the UTC date string,
so the daily board is deterministic and identical for everyone, with no server involved.

**Verified:** 750 generated boards across 5×5, 7×7 and 9×9 — zero failures. Every board had a
complete spanning tree, lit fully in its solved state, was solvable by minimum rotations, contained
no isolated cells, and none started pre-solved.

## Technical

- **31 KB total**, single HTML file, no build step, no dependencies, no framework
- No backend, no accounts, no analytics, no cookies — the board is generated on your device
- `pointerdown` input (no 300 ms mobile tap delay), `touch-action: none` so the page never scrolls mid-play
- Procedural WebAudio (no audio files) and `navigator.vibrate` haptics
- Respects `prefers-reduced-motion`
- Comfortably inside Poki's 8 MB and CrazyGames' 50 MB initial-download limits

## Local development

It is one file. Open `index.html` in a browser, or:

```bash
python -m http.server 8000
```

## Licence

Not yet licensed. All rights reserved for now.
