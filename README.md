# MCM / Package Browser — 2-D layers + 3-D stack

A single-file, dependency-free browser for a flip-chip **BGA-on-substrate** package:

- **2-D layer view** — step through etch layers top-down (visibility toggles, solo, net highlight,
  hover-for-net), pan/zoom.
- **3-D exploded stack** — orbit the buildup (soldermask → metals/planes → core → metals →
  soldermask), die on top, C4 bumps, BGA balls, and vias as pillars. Explode slider + auto-spin.

Ships with a sample FCBGA (19×19 mm, 4 metal layers, 225 balls, 196 bumps). Use **Load JSON** to
view a real design.

## Real `.mcm` files
A Cadence Allegro APD/SiP `.mcm` is a proprietary binary — not parseable in a browser. Export it to
the neutral **package-JSON** schema (documented in-page) via Allegro `extracta` / IPC-2581 / ODB++,
then load it here.

Live: https://borenw.github.io/mcm-browser/ · Part of https://borenw.github.io/ (Page 29)

Built with Claude Code.
