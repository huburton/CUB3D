# Cub3D

*[Version française](README.fr.md)*

A Wolfenstein 3D-style raycaster in C with the MiniLibX: textured walls, keyboard movement and collisions,
from a 2D map. School 19 (42 network) project, built in a pair (logins `msochor` and `huburton`).

![cub3d running: textured corridor rendered by the raycaster](docs/cub3d.webp)

## What it does

- Parses a `.cub` file: four wall textures (`NO`, `SO`, `WE`, `EA`, in `.xpm`), floor and ceiling colours,
  then the map, which must be closed by walls and hold exactly one start position (`N`, `S`, `E` or `W`).
- Casts one ray per column pair (256 rays for a 512-pixel window) across a 60° field of view, with the
  DDA algorithm, and corrects the distance to avoid the fisheye effect.
- Draws each wall slice from the right texture, sampled with fixed-point maths; floor and ceiling are flat colours.
- Moves the player in real time with collisions checked on a radius around them.

```text
NO ./textures/north.xpm        111111
SO ./textures/south.xpm        1000N1
WE ./textures/west.xpm         100001
EA ./textures/east.xpm         111111
F 220,100,0
C 225,30,0
```

## How the raycaster works

1. **Player**: a position in pixels, an angle in radians, movement flags and a collision radius.
2. **Rays (DDA)**: for each ray, a direction vector, a step direction (±1), the delta and initial side
   distances, then a walk through the grid, cell by cell, until a wall is hit.
3. **Projection**: `line_height = HEIGHT / perpendicular_distance`, centred vertically and clamped to the screen.
4. **Texture**: the hit position on the wall gives the texture column; rows are sampled down the slice.

## Controls and build

| Key | Action |
|---|---|
| `W` `S` | move forward / backward |
| `A` `D` | strafe left / right |
| `←` `→` | turn |

```bash
cd CUB3D && make
./cub3D maps/test.cub
```

## Resources

- [Lode's raycasting tutorial](https://lodev.org/cgtutor/raycasting.html)
- [A video explaining raycasting](https://www.youtube.com/watch?v=g8p7nAbDz6Y)

AI was used to better understand the maths behind raycasting.
