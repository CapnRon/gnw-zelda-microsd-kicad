# GnW-Zelda-MicroSD

A castellated-edge microSD interposer for the Game & Watch: The Legend of
Zelda console mod scene -- built as original KiCad artwork, informed by
existing community Gerber-only releases rather than copied from any of
them.

The board mates directly to the console's STM32 mainboard via a
7-position castellated edge (real 0.3x1.2mm SMD pad geometry, machined
flush by the board-edge mill -- this is the critical mechanical/electrical
mating surface, not an approximation) and carries an onboard RT9193-28GB
LDO regulating the console's raw ~4.2V battery rail down to a safe 2.8V
for the microSD card.

## Status

Working draft, not yet fab-verified on real hardware. Electrically clean
(0 unconnected nets, 0 shorts per `kicad-cli pcb drc`) with real routing
and filled ground pours on both copper layers. Remaining DRC output is
DFM-grade (clearances, drill sizes, silk overlap) -- normal manual-review
territory before ordering boards, not electrical faults. See
`docs/design-notes.md` for the full derivation, findings, and everything
still worth double-checking by hand in the KiCad GUI before fabrication.

## Commit history

- **Initial commit** -- the original reconstruction: schematic, PCB,
  footprints, and silkscreen rebuilt from scratch in KiCad from measured
  Gerber geometry (see Origin and attribution below). No third-party
  KiCad source files were used at this point -- every footprint was
  RE'd/approximated by hand from pad positions and sizes read off the
  Gerbers.
- **This commit** -- incorporates real assets from PrimoAngelo's native
  KiCad source (added as source material in the prior commit) now that
  he's confirmed as the known-good original author: J2 (microSD socket)
  and J3 (VBAT tab) were swapped from their RE'd/approximated footprints
  to his real `MSD1A`/`Pad_1x1_5` pad geometry, and a real STEP model was
  wired into U1 (RT9193 LDO). C1/C2/R1 now carry a second, real
  hand-solder-0805 footprint stacked at the same location as their
  original passive footprint, so either package size can be assembled.
  Also fixed a schematic/PCB reference-designator mismatch (J2/J3 and
  U1/U2 were swapped between the two files) and a batch of inconsistent
  net names left over from re-syncing the PCB against the schematic.

## Repository layout

```
hardware/         KiCad project (open GnW-Zelda-MicroSD.kicad_pro here)
production/       Gerbers + drill files (RS-274X, Excellon) + a zipped bundle
docs/             Design notes, top/bottom copper renders
source-material/  Original third-party source files this design descends
                   from, preserved for attribution/licence compliance --
                   see source-material/README.md
LICENSE           CERN-OHL-S v2 full text
NOTICE.md         Licensing rationale and attribution summary
```

## Circuit

- **U1** -- RT9193-28GB LDO. VIN <- raw battery (~4.2V) via the J1
  castellated edge; EN <- console GPIO (via J1); VOUT -> 2.8V microSD
  supply.
- **J2** -- microSD socket (SPI-mode wiring: CS/MOSI/MISO/CLK + VDD/GND),
  PrimoAngelo's real `MSD1A` footprint.
- **J1** -- 7-position castellated edge: GND, CS, MISO, CLK, MOSI, NC
  (spare), EN.
- **J3** -- castellated VBAT solder tab, PrimoAngelo's real `Pad_1x1_5`
  footprint (same machined-edge technique as J1) -- solders directly to
  the positive terminal of an external SMD capacitor.
- **R1** -- 100k pulldown holding the LDO disabled by default until the
  console GPIO actively enables it. Real hand-solder-0805 footprint
  stacked at the same location as the original RE'd footprint.
- **C1/C2** -- input/output decoupling for U1. Same dual-footprint
  stacking as R1 (0805 hand-solder on top of the original).
- **H1-H4** -- plated, electrically-isolated mounting pads (3.0mm pad /
  1.8mm drill), positioned to match the reference designs' mounting
  pattern.

## Origin and attribution

This design was reconstructed from precise geometry (board outline, pad
positions/sizes, net connectivity, and real copper routing) extracted
directly from Gerber-only community releases for the same G&W Zelda
microSD mod, tracing back through three generations of prior work:

- **Tim Schuerewegen** originated the project.
- **Hundshamer** picked it up and pushed it forward to its final form,
  `GNW-MICROSDx1-ZELDA-V3.4`. Used during reconstruction for
  cross-validation (confirmed identical castellated pinout via a
  different edge-connector pin-numbering scheme than PrimoAngelo's
  release) and as the source for the drilled-via castellation technique
  referenced during development.
- **PrimoAngelo** began his own work from Hundshamer's v3.4 design,
  producing `MicroSD_Zelda_FORO_JLC_FIX_V2 - NO_LOGO`, credited to him in
  its own back-silkscreen (visible in the source Gerbers). This is the
  primary geometric basis for this repo's outline, placement, and
  routing.

Everything in this repository (schematic, PCB, footprints, silkscreen)
was rebuilt from scratch in KiCad from measured geometry, not copied.
This reconstruction was completed before any contact with any of the
above. Afterward, both PrimoAngelo and Hundshamer were reached directly:
PrimoAngelo confirmed authorship and provided native KiCad source for
this design and a sibling Mario board; Hundshamer confirmed the lineage
back to Schuerewegen's original project in a friendly exchange, but had
no additional source material to contribute beyond what was already used
above. All original source material received is preserved under
`source-material/` for full provenance and CERN-OHL-S v2 compliance; see
`source-material/README.md` for the complete attribution chain and
`NOTICE.md` for the licensing rationale. Full credit to the original
designers for the mod itself; if you're one of them and want anything
here changed or removed, open an issue.

## License

CERN-OHL-S v2 (CERN Open Hardware Licence Version 2 - Strongly
Reciprocal). See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md).

## Building / opening

Requires KiCad 10. Open `hardware/GnW-Zelda-MicroSD.kicad_pro`.

Most custom parts (castellated connectors, exact-geometry footprints for
the LDO/passives) are embedded directly in the `.kicad_pcb`. J2 and J3
now use real footprints from PrimoAngelo's native library, referenced via
project-scoped libraries in `hardware/fp-lib-table` that point into
`source-material/` with `${KIPRJMOD}`-relative paths -- so the project
stays portable to any clone location, but `source-material/` must come
along with it. The schematic symbol library
(`hardware/gnw_zelda_microsd.kicad_sym`) is referenced the same way.

## Before ordering boards

- This is a hand-routed/reconstructed draft -- give it a real pass in the
  KiCad GUI (visual DRC review, silk/courtyard cleanup) before sending to
  a fab.
- The castellated edge geometry is the one dimension that matters most:
  verify pad pitch/size against your actual console board before
  committing to a fab run.
- No BOM/pick-and-place export included yet -- add before ordering
  assembly.
