# DiscreteOpAmp

An op amp built from discrete transistors, done as a learning exercise: two discrete op-amp designs of increasing complexity, a reference redraw of the classic LM741 internals, and a companion function-generator board to drive them. Designed in Altium Designer 23.10.

## What's in the repo

There are two Altium projects.

### `DiscreteOpAmp.PrjPcb` — the op-amp board

A hierarchical project with `main.SchDoc` at the top and one sheet symbol per design:

| Sheet | What it is |
| --- | --- |
| `BasicDiscreteOpAmp.SchDoc` | The minimal four-transistor op amp: a 2N3904 long-tailed-pair input stage (Q1/Q2, resistive tail), a 2N3906 gain stage (Q3) with a 100 pF Miller compensation cap, and a 2N3904 emitter-follower output (Q4). Reference: `BasicDesign_Refschematic.png`. |
| `AdvancedDiscreteOpAmp.SchDoc` | The improved design: the differential pair gets a current-mirror active load (Q9/Q10) and a ~1 mA current-sink tail (Q11/Q12), the gain stage is biased by a ~10 mA current sink (Q13/Q14) with 30 pF compensation, and the output is a diode-biased class-AB push-pull pair (Q8/Q15) with 220 Ω emitter resistors. Runs from ±9 V. Reference: `ImprovedDesign_Refschematic.png`. |
| `LM741_Design.SchDoc` | A redraw of the LM741's transistor-level schematic, kept as a reference. It is not instantiated in the hierarchy and is not on the PCB. Reference: `Documentation/LM741_DiscreteDesign.png`. |

`PCB1.PcbDoc` is the board, roughly 50 × 67 mm, two layers. Fabrication outputs (Gerber X2 and NC drill, plus a zip of them) are in `Project Outputs for DiscreteOpAmp/`.

### `FuncGen/FuncGen.PrjPcb` — the function generator

A test-signal board to drive the op amps: an LMC555 timer and an LMC6484 quad CMOS op amp (the sine path follows the Bubba-oscillator / TI sine-generator references in `Documentation/`), with Bourns 3214 10 kΩ multiturn trimmers for adjustment, Keystone 476 banana jacks for I/O, and a Hirose DF9 15-position 1 mm board-to-board connector (`Connectors.SchDoc`). The board is roughly 109 × 66.5 mm, two layers. Its component library is `FuncGen/FuncGen.IntLib` and its fabrication outputs are in `FuncGen/Project Outputs for FuncGen/`.

The root-level `Oscillator.SchDoc` and `Connectors.SchDoc` are not part of either project. They differ from the versions under `FuncGen/`, which are the ones the function-generator project uses; they look like leftovers from before it moved into its own folder.

### Reference material

- `Datasheets/` — LMC555, LMC6484, the Bourns and Vishay trimmers.
- `DF9_Catalog_D31692_en-2301602.pdf` — Hirose DF9 connector catalogue.
- `Documentation/` — LM741 internal schematic, Bubba oscillator notes, TI sine-wave generator app note (SLOA278).

## Opening it

Open `DiscreteOpAmp.PrjPcb` or `FuncGen/FuncGen.PrjPcb` in Altium Designer 23.10 or later. The PCBs carry footprint parameters, which older versions drop on open.

## Status

Both boards have Gerbers generated, but neither has been confirmed as built and tested, so treat the outputs with care:

- **The op-amp board's Gerbers are older than its schematics.** They were generated on 2023-10-20; the basic and advanced circuits were completed on 2023-10-21 and the project was reorganised into a hierarchy on 2024-02-23. Regenerate after checking the PCB is in sync with the schematics before ordering.
- **The function generator's committed DRC report shows open issues.** The report from 2023-10-23 16:36 lists 107 rule violations, including 2 short circuits and 5 unrouted nets. The Gerbers were generated about an hour later, so these may since have been fixed, but there is no newer DRC report in the repo to confirm it. Re-run DRC before fabricating.
- No simulation results, measurements or BOM are in the repo yet.

## History

- 2023-10-20 — project started; first PCB outputs.
- 2023-10-21 — basic and advanced discrete op amps complete.
- 2023-10-23 — function generator developed.
- 2023-10-24 — Gerbers generated, documentation added.
- 2023-11-08 — Altium history/preview/log folders git-ignored.
- 2024-02-23 — schematics moved into a hierarchy under `main.SchDoc`; LM741 reference sheet added.
