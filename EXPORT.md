# Getting package data out of a Cadence `.mcm`

**Short answer:** a `.mcm` (Allegro APD / SiP database) is a **proprietary binary** — Cadence does not publish the
format, so there is **no open-source parser and no legitimate online converter**. Anything that reads it has to
go *through* Allegro. The good news: you already have the `.mcm`, so you have Allegro, and Allegro can export
several neutral formats this browser (or a tiny converter) can read.

Pick whichever you can produce, export it, and either load it directly (if it's already the package-JSON) or
run a small converter to the [package-JSON schema](./index.html) the browser expects.

---

## Route A — IPC-2581 (recommended, easiest)
An **open, documented XML** standard — the most robust thing to convert.

- Allegro PCB Editor / APD → **File ▸ Export ▸ IPC-2581**.
- Produces one `.xml` (or `.cvg`) with layers, nets, pads, vias, component pins, and outlines.
- Convert XML → package-JSON with a small script (see "Converter", below).

## Route B — ODB++
Also complete (copper, drills, netlist, component/pin data), as a directory/zip of ASCII.

- Allegro → **File ▸ Export ▸ ODB++** (needs the ODB++ export option).
- Parse the `odb/steps/<step>/layers/...` + `eda/data` + `components` files → package-JSON.

## Route C — `extracta` (scriptable, no GUI)
`extracta` ships with Allegro and dumps ASCII reports from a `.brd`/`.mcm` given an **extract view** (control file):

```sh
extracta  design.mcm  pkg.ctrl  pkg.txt
```

The control file lists the record types + fields to pull (pins with `PIN_X/PIN_Y/NET_NAME/SUBCLASS`, vias,
nets, layer names, etc.). Allegro ships sample extract views under
`<cadence_install>/share/pcb/extract/` — start from those. Then convert the columnar `pkg.txt` → package-JSON.

## Route D — SKILL / AXL dump (most direct)
A short SKILL script run inside APD walks the DB (`axlDBGetDesign()`, nets, pins, vias, layers) and can write
the package-JSON **directly** — no post-conversion. This is the cleanest if you're comfortable running SKILL in
the Allegro command window. (Ask and a tailored `mcm_to_json.il` can be written for your Allegro version.)

---

## Converter
Once you have an export (IPC-2581 XML / ODB++ / extracta ASCII), a ~100-line Python script maps it to the
package-JSON: `stack[]` (etch layers top→bottom), `die`, `bumps[]`, `balls[]`, `vias[]` (with `from`/`to`
layers), and per-layer `planes/traces/pads`. Coordinates in **mm, origin at package center**.

**Fastest way to get a working, exact converter:** produce one export (IPC-2581 is one menu click) and share a
small snippet — the converter can then be written and tested against your real field names, since exact tokens
vary by Allegro version and export options.

## What you can't do
- Parse the binary `.mcm`/`.brd` directly (in a browser or otherwise) without Allegro — no open parser exists.
- Find an online `.mcm → anything` service — the format is closed; don't upload proprietary designs to random
  sites regardless.
