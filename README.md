# Battery-Protected Multi-Rail Regulator PCB

KiCad project for a battery-powered linear regulator board that generates two
regulated rails (5V and 3.3V) from a single-cell battery input, with reverse-polarity
protection and a power-on indicator.

## Overview

- **Input:** Single-cell battery via a 2-pin JST PH connector (J1)
- **Protection:** Series Schottky diode (D1, 1N5820) blocks reverse-polarity connection
- **Outputs:** Two independently regulated rails
  - 3.3V via U1 (LDL1117S33R)
  - 5.0V via U2 (LDL1117S50R)
- **Indicator:** Power-on LED (D2, LTST-C171KGKT) with current-limiting resistor (R1, 255Ω)
- **Breakout:** 4-pin header (J3) exposing BAT / 5V / 3.3V / GND for downstream boards
- **Layout:** 2-layer PCB, fully routed (0 unrouted nets), DRC clean (0 errors)

## Schematic

| Ref | Part | Function |
|---|---|---|
| U1 | LDL1117S33R | 3.3V fixed LDO regulator (SOT-23-5) |
| U2 | LDL1117S50R | 5.0V fixed LDO regulator (SOT-23-5) |
| D1 | 1N5820 | Schottky diode, reverse-polarity protection |
| D2 | LTST-C171KGKT | Power-on indicator LED |
| R1 | 255Ω | LED current-limiting resistor |
| C1, C2 | 1µF (0805) | Input decoupling caps |
| C3, C4 | 4.7µF (0805) | Output decoupling caps |
| J1 | B2B-PH-K-S | 2-pin battery input connector |
| J2 | PC712A | Auxiliary connector/switch |
| J3 | Conn_01x04 | 4-pin breakout header (BAT/5V/3.3V/GND) |

Full details in [`hardware/BOM.csv`](hardware/BOM.csv).

## Repository structure

```
.
├── README.md
├── LICENSE
├── .gitignore
├── hardware/
│   ├── VLO.kicad_pro
│   ├── VLO.kicad_sch
│   ├── VLO.kicad_pcb
│   └── BOM.csv
├── production/
│   ├── gerbers/
│   ├── BOM.csv
│   └── pos.csv
└── images/
    ├── schematic.png
    ├── pcb-layout.png
    └── pcb-3d.png
```

Drop your `.kicad_pro` / `.kicad_sch` / `.kicad_pcb` files into `hardware/`, and export
Gerbers + a top-view PCB screenshot into `production/` and `images/` respectively.

## Design notes

- D1 sits in series between the battery connector and the BAT rail, so a reversed
  battery simply doesn't power the board instead of damaging U1/U2.
- Input caps are 1µF and output caps are 4.7µF per regulator, per the LDL1117
  datasheet's minimum stability requirements.
- DRC was run with **0 errors**; a small number of warnings remain (silkscreen
  clearance on densely packed 0805 footprints) and are cosmetic, not functional.

## Status

- [x] Schematic capture
- [x] Footprint assignment
- [x] 2-layer routing (0 unrouted nets)
- [x] DRC (0 errors)
- [ ] Gerbers exported / fab-ready
