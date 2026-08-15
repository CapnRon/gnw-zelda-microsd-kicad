# Source material

This directory preserves, unmodified, every original design file this
project's attribution chain depends on -- kept here for provenance and
CERN-OHL-S v2 licence compliance (Covered Source must be made available;
see the repository [LICENSE](../LICENSE) and [NOTICE.md](../NOTICE.md)).

Each subdirectory contains the original archive exactly as received
(`*.zip`) plus its extracted contents (`extracted/`) for easier browsing
and diffing.

## Attribution chain

1. **Tim Schuerewegen** originated the Game & Watch Zelda microSD mod
   project this whole lineage descends from.
2. **Hundshamer** picked up the project and pushed it forward to its
   final form, `GNW-MICROSDx1-ZELDA-V3.4` -- see
   `gnw-microsd-zelda-v3.4-hundshamer/`.
3. **PrimoAngelo** began his own work from Hundshamer's v3.4 design,
   producing a derivative/alternate release -- see
   `microsd-zelda-gerbers-primoangelo/` (the Gerber-only file this
   repository was reverse-engineered from) and `kicad-zelda-primoangelo/`
   (PrimoAngelo's own native KiCad source for the same design, obtained
   later).
4. **This repository** was reverse-engineered from PrimoAngelo's
   Gerber-only release, then cross-validated against Hundshamer's v3.4
   design, before any contact with any of the above.

After that reconstruction was complete, both PrimoAngelo and Hundshamer
were contacted directly. PrimoAngelo confirmed authorship and provided
native KiCad source for this design and a sibling Mario board (see
below). Hundshamer confirmed the lineage back to Schuerewegen's original
project in a friendly exchange, but had no additional source material to
contribute beyond `GNW-MICROSDx1-ZELDA-V3.4`, already included here.

## `gnw-microsd-zelda-v3.4-hundshamer/`

`GnW_SD_v2.zip`, containing `GNW-MICROSDx1-ZELDA-V3.4` -- a Gerber/
Excellon-only release by **Hundshamer**, who picked up Tim Schuerewegen's
original Game & Watch Zelda microSD mod project and pushed it forward to
this, its final v3.4 form. Used during the original reverse-engineering
work for cross-validation (it confirmed the castellated pinout via a
different edge-connector pin-numbering scheme than PrimoAngelo's release)
and as the source for the drilled-via castellation technique referenced
in `docs/design-notes.md`.

## `microsd-zelda-gerbers-primoangelo/`

`MicroSD_Zelda_Final.zip`, containing
`MicroSD_Zelda_FORO_JLC_FIX_V2 - NO_LOGO` -- a Gerber/Excellon-only
release (no native KiCad project) credited to **PrimoAngelo** in its own
back-silkscreen, who began his own work from Hundshamer's v3.4 design
above. This is the file this repository's design was originally
reverse-engineered from, before any contact with any of the original
designers -- see `docs/design-notes.md` for the full reverse-engineering
methodology.

## `kicad-zelda-primoangelo/`

`KICAD_ZELDA_PrimoAngelo.zip`, containing the native KiCad project
(`MicroSD_Zelda_FORO_JLC_FIX_V2.kicad_pcb`/`.kicad_pro`) for the same
design above. Provided directly by PrimoAngelo **after** this
repository's reverse-engineering work was already complete, when the
designer was contacted for permission/attribution. Included here for full
provenance and licence compliance; this repository's own KiCad files were
not re-derived from it, since the reconstruction from Gerbers was already
finished and independently verified by the time this arrived.

## `kicad-mario-primoangelo/`

`KICAD_MARIO_PrimoAngelo.zip`, containing a native KiCad project
(`MicroSD_Mario_JLC_FIX_V3.kicad_pcb`/`.kicad_pro`) for a sibling
microSD mod board for a different Game & Watch title (Super Mario Bros.),
also by PrimoAngelo. Provided for the same reason as above, during the
same contact. Not a source for this repository's Zelda board -- included
purely for attribution/provenance and as reference material for anyone
working on the Mario variant in the future.

## `kicad-libs-primoangelo/`

`Libs.zip`, containing PrimoAngelo's own native KiCad footprint libraries
and 3D models used to build the designs above: `Impronte/` ("footprints",
Italian) with `Pad.pretty`/`_FG_Pad.pretty` (the castellated edge-contact
footprints, including `Pad_0_3x1_2.kicad_mod` -- matching this
repository's own independently-reconstructed 0.3x1.2mm castellated pad
geometry exactly, real confirmation the original reverse-engineering in
`docs/design-notes.md` got the geometry right) and `_FG_MicroSD.pretty`
(microSD socket/connector footprints), plus `Modelli_3D/` ("3D models")
with STEP models for the microSD socket and connector. Provided by
PrimoAngelo in a later contact, for the same attribution/provenance
reason as the items above -- not used to modify this repository's
existing (already-verified) footprints, included for completeness and as
a resource for future work (e.g. 3D visualization, mechanical checks).

## Licensing note

These files are third-party work, included here under the terms of
CERN-OHL-S v2 (the licence this repository itself is released under --
see [NOTICE.md](../NOTICE.md)). They are not covered by this repository's
own attribution disclaimers in `README.md`; refer to the original
designers for any use beyond what CERN-OHL-S v2 permits.
