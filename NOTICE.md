# Notice

This project is licensed under the **CERN Open Hardware Licence Version 2
- Strongly Reciprocal (CERN-OHL-S v2)**. See [LICENSE](LICENSE) for the
full licence text.

## Why CERN-OHL-S v2

This board's design lineage traces back through three generations of
prior open work (see `source-material/README.md` for the full chain):
Tim Schuerewegen originated the project, Hundshamer picked it up and
pushed it forward to its final form (`GNW-MICROSDx1-ZELDA-V3.4`), and
PrimoAngelo began his own work from there, producing the release this
repository's PCB and schematic were independently reconstructed from via
reverse engineering (see `docs/design-notes.md`) -- before any contact
with any of the three.

After that reconstruction was complete, the author reached both
PrimoAngelo and Hundshamer. PrimoAngelo confirmed authorship and provided
native KiCad source for both the Zelda board this repository covers and a
sibling Mario board, for attribution purposes. Hundshamer confirmed the
lineage back to Schuerewegen's original project in a friendly exchange,
but had no additional source material to contribute beyond what was
already used for cross-validation. CERN-OHL-S v2 was chosen for this
repository at that point, matching the reciprocal, source-available
spirit of the designs it descends from, and all provided source material
is preserved in `source-material/` per the licence's Source-availability
requirements (Section 3), even though most of it was received after this
repository's own design work was finished.

## Attribution

- **Tim Schuerewegen** -- originated the Game & Watch Zelda microSD mod
  project this lineage descends from.
- **Hundshamer** -- picked up the project and pushed it forward to its
  final form, `GNW-MICROSDx1-ZELDA-V3.4`.
- **PrimoAngelo** -- began his own work from Hundshamer's v3.4 design,
  producing `MicroSD_Zelda_FORO_JLC_FIX_V2`, the Gerber-only release this
  repository's design was reverse-engineered from, and later provided
  native KiCad source for that design and a sibling Mario board, both
  included under `source-material/`.

Full detail on what was derived from what, and how, is in
`source-material/README.md` and `docs/design-notes.md`. If any of the
above would prefer different credit or wish to have anything changed or
removed, open an issue.

## Covered Source

Per CERN-OHL-S v2 Section 2, the copyright notice and this Notice file
apply to all files in this repository under `hardware/`, `production/`,
and `docs/`, and to the third-party Source preserved under
`source-material/` (itself further governed by the terms noted in
`source-material/README.md`).
