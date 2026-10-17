# Design notes

## 1. Vibration module (pushrod / crank)

```
   stationary upright (T-slot)
        │
        ├── toothbrush BODY taped to upright (does not move)
        │
        └── shaft tip ──► pressed into a hole in the hanging tray frame
                            │
                    tray hangs on soft suspension (pipe cleaners today,
                    printed leaf springs next) → free to oscillate
                            │
                    tilt ~3–5°: shim under the BRUSH-end upright so
                    material walks away from the motor
```

- Sonic brush: ~250 Hz, ~1 mm stroke at the tip. Enough to sieve 1 mm fines through a 2 mm mesh and to walk 6 mm rice.
- Direction of walk follows the shaft's push direction. Rotate the brush body until the walk is straight; a diagonal walk means the shaft is off-axis.
- Don't tape the body to the tray: the handle is the counter-mass, the head is what moves.

## 2. Honeycomb singulation layer

Stack the kit's hex grid on top of the screen for a given fraction. Cell size ≈ 1.3× the fraction's max dimension, depth ≈ 0.8× its height, so one specimen seats and a second rides over the rim and walks off.

Printed variants for next week (STLs to Sean):

| Fraction | Cell across flats | Cell depth |
|---|---|---|
| 2–6 mm (rice) | 8 mm | 4 mm |
| 6–10 mm (wild rice) | 13 mm | 6 mm |
| 10–20 mm (broken noodle) | 26 mm | 10 mm |

Reference: zebrafish embryo arraying templates (JoVE 2018) — same pocket-self-loading principle.

## 3. Control loop → MCP server `sieve-stack`

Mirrors the digital-twin verbs on the event page.

```python
from mcp.server.fastmcp import FastMCP
mcp = FastMCP("sieve-stack")

@mcp.tool()
def inspect() -> dict:
    """State: which screen/honeycomb is loaded, per-cell occupancy from the webcam
    (0 / 1 / 2+), blinded flag, last frame path."""

@mcp.tool()
def verify(plan: list[dict]) -> dict:
    """Dry-run a plan (shake(seconds, duty), tilt(deg), clear()) against the state model.
    Returns pass/fail + reason (no screen, already singulated, tilt out of range)."""

@mcp.tool()
def dispatch(plan: list[dict]) -> str:
    """Re-verify, run on hardware (ESP32 relay to the motor, or human-in-the-loop prompt).
    Returns run_id."""

@mcp.tool()
def trace(run_id: str) -> dict:
    """Provenance dossier: steps, parameters, per-cell counts before/after,
    frames with timestamps, firmware version. Exceptions list (cells with 0 or 2+)
    is handed to the arm."""
```

"Done" = every cell reads exactly 1, or the exception list is short enough to hand to the arm.

## 4. BOM beyond the kit

| Item | ~Cost |
|---|---|
| Sonic toothbrush / 250 Hz vibration motor | $10 |
| USB webcam, 1080p | $20 |
| LED panel for even top light | $15 |
| ESP32 + relay (motor on/off, duty) | $8 |
| Printed leaf springs, honeycomb variants | filament |
