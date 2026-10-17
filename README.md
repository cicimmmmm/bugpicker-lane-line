# BUZZ — a self-driving sieve for step 1 of BioKEA's sample prep

**Team BUZZ: Patrick Timothy Hanley · Cici Xi Mao**
Physical AI for Science · Impact Lab · Studio 45, San Francisco · Oct 17, 2026 · Track A (BioKEA)

![Rig overview](media/01-rig-overview.jpg)

## The problem (Track A brief, slide 11)

BioKEA's sample prep goes from a bag of preserved insects to barcodes in four steps:

| Step | What | Instrument |
|---|---|---|
| **1 · ours** | **Sort by size** | **BugSorter sieve — linear actuator + water jets today; clogs and stalls on real preserved samples** |
| 2 | Disperse | BugDisperser — spread the fraction so specimens sit alone |
| 3 | Pick to plate | BugPicker — vision-guided suction into a 96-well plate |
| 4 | Lyse and barcode | Enzyme dispenser → incubator → sequencing |

We did not change the steps. We took **step 1** — the brief's first ask, *"a more robust robotic sieve that works on real, preserved samples, run after run"* — and asked what the simplest robust drive for it could be. The kit's open question (slide 14): *what motion clears a blinded screen, and how does the stack know it is done?*

Today's stand-in for bugs is the pantry size ladder: chia (~1 mm), couscous (~2 mm), rice (~6 mm), wild rice (~10 mm), broken noodles (10–20 mm), whole noodles (~30 mm).

## The idea

One vibration source does three jobs on the existing printed sieve:

1. **Grades by size.** A sonic toothbrush (~250 Hz, ~$10) acts as a pushrod on a hanging sieve tray. Fines fall through; the mesh stays clear while it sorts — nothing to time, no pump.
2. **Walks the retained fraction** along a slope toward the hand-off, like an industrial vibratory feeder.
3. **Settles the sample** into the kit's hex honeycomb, one specimen per cell, so step 2/3 receive specimens already apart. A camera overhead counts 0 / 1 / 2 per cell — that is how the stack knows it is done.

```
 bag → [ step 1: vibrating sieve + honeycomb + camera count ]  → step 2 disperse → step 3 pick → step 4 barcode
               ^ this repo
```

## What we built today (kit parts + under $15)

| Part | Source |
|---|---|
| Printed sieve tray + 2 mm mesh, square tray frame, round sieve ring + perforated plate, hex honeycomb grid, T-slot extrusion | BioKEA kit |
| Battery sonic toothbrush (vibration source, pushrod) | owned, ~$10 |
| Rubber bands / pipe cleaners (soft suspension), painter's tape, paper plate (catch) | pantry |

Mounting that worked: toothbrush **body fixed** to a stationary upright; the vibrating **shaft tip pressed into a Ø2.8 mm hole in the hanging tray frame** (crank/pushrod). Taping the body rigidly to the tray does not work — the handle is the counter-mass; only the head moves.

![Top view](media/02-rig-top.jpg)
![Honeycomb on ring](media/03-honeycomb-ring.jpg)

## Results (honest, dry run with grains)

| Step | Status | Evidence / next move |
|---|---|---|
| Grade by size through the mesh | ✅ works | chia/sesame dropped into the ring; rice stayed on top; nothing crushed or stuck |
| Walk specimens in one direction | 🟡 partial | rice moved to a corner, not an edge — rotate the drive 30–45° to align the push with the rails |
| One specimen per cell (honeycomb) | ⏳ not yet run | insert is printed and on the table; cells fit one grain |
| Count cells from one photo | ⏳ not yet run | phone overhead; Claude returns 0 / 1 / 2 per cell plus exceptions |

Demo video: [`media/demo-shake.mp4`](media/demo-shake.mp4)

## Why it is better — step 1, same parts

| | Original BugSorter | BUZZ |
|---|---|---|
| Drive | linear actuator shakes, water jets clear | one toothbrush on a hung frame, 100+ Hz through one hole |
| Moving parts | motor, pump, valves, controller | one brush, one switch |
| Clearing a blinded mesh | water jet, timed | vibration keeps the mesh clear while it sorts |
| Done detection | not addressed | one overhead photo: nothing left above the mesh, or cells read 0 / 1 / 2 |
| Hand-off to step 2/3 | heap on a tray | honeycomb: one specimen per cell |

Not yet proven: wet operation in ethanol (damping changes), and drive-direction alignment.

## Next weekend (ReBot arms arrive)

- STLs to Sean tonight: lane plate, honeycomb variants per sieve fraction (2 / 6 / 10 mm cells), printed leaf springs to replace rubber bands.
- Wet run with real preserved specimens from BioKEA's waste sample.
- Sonic drive (Sonicare pin, ~260 Hz); camera count wired into an MCP server (`inspect / verify / dispatch / trace`) so every run writes its own record.
- BOM ask under $200: sonic brush, USB camera, LED panel, PETG, T-nuts.

See [`docs/design.md`](docs/design.md) for the pushrod mount, honeycomb cell sizes, MCP tool schema, and BOM.

Thanks to Sean, Michelle, Chris and Austin at BioKEA for the kit and the scoping.
