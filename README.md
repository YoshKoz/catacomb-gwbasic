# Catacomb of the Silver Key

A first-person raycast dungeon crawler in a single GW-BASIC source file.

No extensions, no machine-code stubs, no external data — everything (map, sprites,
music, monster tables) lives in `DATA` statements inside `CATACOMB.BAS`.

## Run it

```
pcbasic CATACOMB.BAS          # PC-BASIC (pip install pcbasic)
```

or under DOSBox with a real `GWBASIC.EXE`:

```
GWBASIC CATACOMB.BAS
```

## Controls

| key | action |
|-----|--------|
| `W` / Up | step forward |
| `S` / Down | step back |
| `A` `D` / Left Right | turn |
| `Q` `E` | strafe |
| `SPACE` | attack the square ahead |
| `G` | grab item underfoot |
| `P` | quaff a potion |
| `M` | automap |
| `>` | descend the stairs |
| `F1` | help |
| `1` / `2` | save / load `CATACOMB.SAV` |
| `X` | quit |

## Engine

* **Raycaster** — DDA over the 32x32 grid, 40 columns of 8px, 60 degree FOV,
  perpendicular-distance correction, per-column Z-buffer. Column ray directions
  are precomputed once at start-up and rotated by the facing vector, so there is
  no trigonometry in the render loop.
* **Shading** — three light bands plus a face-orientation darkening step, with
  vertical dither stripes for the far band. Band thresholds are driven by torch
  fuel, so the view closes in as the torch burns down.
* **Sprites** — billboarded monsters and items projected through the camera
  matrix, rasterised from 8x8 bitmasks scaled to distance, occluded per-block
  against the wall Z-buffer.
* **Dungeon generation** — non-overlapping rectangular rooms connected by
  L-shaped corridors, with doors punched where a corridor pierces a wall line,
  then items and monsters scattered onto free floor.
* **Monster AI** — a depth-limited breadth-first Dijkstra flow field is rebuilt
  around the player each turn; monsters descend the gradient, blocking each other
  and attacking on adjacency. They stay dormant until a DDA line-of-sight check
  and a range test wake them.
* **GW-BASIC features on show** — `DRAW` and `PLAY` macro strings, `ON PLAY()`
  event trapping to keep the background music queue fed, `ON KEY()` for the help
  screen, `ON ERROR` for disk faults, `PRINT USING` for the HUD, `WRITE #` /
  `LINE INPUT #` for the save file.

Six levels. The Bone Dragon is on the sixth.
