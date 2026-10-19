# DiscreteOpAmp

An op amp built from discrete transistors, done as a learning exercise: two discrete op-amp designs of increasing complexity alongside an IC op amp for comparison, the classic LM741 internals for reference, and a companion function-generator board to drive them. Designed in Altium Designer 23.10.

## What's in the repo

There are two Altium projects.

### `DiscreteOpAmp.PrjPcb` — the op-amp board

A hierarchical project with `main.SchDoc` at the top. The top sheet puts three op amps side by side, each brought out to its own banana jacks (IN+, IN−, OUT): the two discrete designs as sheet symbols, and a TLV271 IC op amp drawn directly on the top sheet for comparison. Supplies come in as VCC and VEE.

| Sheet | What it is |
| --- | --- |
| `BasicDiscreteOpAmp.SchDoc` | The minimal four-transistor op amp: a 2N3904 long-tailed-pair input stage (Q4/Q3, 1 kΩ collector loads, 1 kΩ tail R7), a 2N3906 gain stage (Q1) with 100 pF Miller compensation (C1), and a 2N3904 emitter-follower output (Q2, 1 kΩ pull-down R6). Reference: `BasicDesign_Refschematic.png`. |
| `AdvancedDiscreteOpAmp.SchDoc` | The improved design: the 2N3904 differential pair (Q9/Q8) gets a 2N3906 current-mirror active load (Q5/Q6) and a current-mirror tail sink (Q12/Q14, set by R10 = 22 kΩ); the 2N3906 gain stage (Q7, 100 pF compensation C2) is loaded by a second current-mirror sink (Q13/Q15, set by R11 = 2.2 kΩ); and the output is a class-AB push-pull pair (Q10/Q11) biased by two diodes (D1/D2), with 220 Ω emitter resistors. Reference: `ImprovedDesign_Refschematic.png`, which uses different designators and a 30 pF cap. |
| `LM741_Design.SchDoc` | The LM741's transistor-level schematic as an embedded image (the same picture as `Documentation/LM741_DiscreteDesign.png`), not drawn circuitry. It is not instantiated in the hierarchy and is not on the PCB. |

`PCB1.PcbDoc` is the board, roughly 50 × 67 mm, two layers. Fabrication outputs (Gerber X2 and NC drill, plus a zip of them) are in `Project Outputs for DiscreteOpAmp/`.

### `FuncGen/FuncGen.PrjPcb` — the function generator

A test-signal board to drive the op amps. An LMC555 runs as a variable-frequency square-wave generator, with its frequency set by a 100 kΩ multiturn trimmer (the sheet notes about 1 kHz to 1 MHz). An LMC6484 quad CMOS op amp then shapes that square wave: an integrator turns it into a triangle, two more stages filter that into a sine, and the fourth section buffers the sine output. The square, triangle and sine outputs leave on a Hirose DF9 15-position 1 mm board-to-board connector (`Connectors.SchDoc`), and supply comes in on Keystone 476 banana jacks. The Bubba-oscillator and TI sine-generator notes in `Documentation/` are background reading; neither circuit is used. The board is roughly 109 × 66.5 mm, two layers. Its component library is `FuncGen/FuncGen.IntLib` and its fabrication outputs are in `FuncGen/Project Outputs for FuncGen/`.

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
- **The top sheet is not annotated.** Its jacks and the TLV271 still carry `J?` and `U?` designators.
- **`AdvancedDiscreteOpAmp.SchDoc` has a stray wire off the sheet**, a short vertical segment at about X = −1000 (left of the sheet edge). It does not appear to connect to anything and can probably be deleted.
- No simulation results, measurements or BOM are in the repo yet.

## History

- 2023-10-20 — project started; first PCB outputs.
- 2023-10-21 — basic and advanced discrete op amps complete.
- 2023-10-23 — function generator developed.
- 2023-10-24 — Gerbers generated, documentation added.
- 2023-11-08 — Altium history/preview/log folders git-ignored.
- 2024-02-23 — schematics moved into a hierarchy under `main.SchDoc`; LM741 reference sheet added.
