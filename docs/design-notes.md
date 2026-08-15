# Design notes

## Goal

Build original KiCad artwork for a G&W Zelda microSD mod interposer,
using two existing Gerber-only community releases as dimensional/circuit
reference (not to be copied wholesale) -- see README for attribution.

## Reverse-engineering methodology

Both source projects are Gerber+drill exports only, no native KiCad
project files. Everything used here was extracted directly from the raw
RS-274X/Excellon data:

- **Board outline**: parsed all `D01`/`D02` draw commands from
  `*-Edge_Cuts.gbr`, chained line/arc segments into closed loops by
  matching endpoint coordinates (tolerance ~0.02mm). The
  `MicroSD_Zelda_FORO_JLC_FIX_V2` source outline resolved to two loops --
  the main board profile (81 segments) and a separate ~4x8mm cutout
  (15 segments) that turned out to be a real hole in the board, not a
  microSD card slot as first assumed.
- **Component placement & pinout**: KiCad's Gerber X2 export embeds
  `%TO.P,<ref>,<pin>,<name>%` and `%TO.N,<netname>%` attributes on every
  pad flash. Parsed these directly to get exact pad positions AND their
  real net/pin names -- no guessing required for pinout mapping.
- **Real pad geometry**: matched each pad flash to its active Gerber
  aperture (`%ADD..%` definitions) to get true pad shapes/sizes, rather
  than assuming standard library footprint dimensions. This caught a real
  early mistake -- initial footprints used guessed sizes (e.g. 0.7x0.5mm
  for the microSD socket signal pads) that were significantly smaller
  than the source's real 0.7x1.6mm pads.
- **Routing**: built a graph from every `F_Cu`/`B_Cu` trace segment
  (nodes = endpoints within ~0.02mm, edges = segments with their real
  layer/width), then ran Dijkstra between each net's known pad endpoints
  to extract the actual routed path -- including real F.Cu/B.Cu layer
  transitions -- rather than inventing new routing from scratch.

## The castellated edge (critical dimension)

The J1 connector butts directly against the console's STM32 mainboard --
this is the one piece of geometry that has to be exactly right, since a
wrong pitch/size means the board simply doesn't mate.

Confirmed via aperture lookup: **0.3mm x 1.2mm rectangular SMD pads**
(Gerber aperture `R,0.300000X1.200000`), each pad centered ~0.55mm in
from the true board edge so it slightly overhangs and gets milled flush
during fabrication -- this is the standard "castellated by copper
overhang" technique (as opposed to the alternative drilled-via
castellation method used by the other reference design, `GNW-MICROSDx1-
ZELDA-V3.4`, which drills a small via exactly on the edge line and relies
on the edge mill to bisect it).

7 contacts, confirmed identical function across both independently-
authored source designs (different edge-connector pin-numbering schemes,
same signals): **GND, CS, MISO, CLK, MOSI, NC (spare), EN**.

## J3: not a through-hole pad

Initially modeled J3 (the VBAT tap) as a simple 1mm through-hole point for
hand-soldering a flying lead. This was wrong. Its net auto-name in the
source Gerber (`Net-(J8-Pin_1)`) revealed it's actually an **8th
castellated contact** whose reference designator was stripped when the
source file was sanitized for public release (filename literally says
"NO_LOGO"). Confirmed via aperture lookup (`R,1.500000X1.500000`, a
1.5x1.5mm square pad) and via position -- it sits right at the board's
left edge with the same small mill-overhang pattern as the J1 contacts.
Rebuilt as a castellated square pad; per the design intent, it's meant to
solder directly to the positive terminal of an external SMD capacitor.

## Ground pour: the "covers everything until refilled" bug

`kicad-cli pcb export gerbers --check-zones` fills zones **transiently**
for that one export -- it never writes the computed fill back into the
`.kicad_pcb` file. Every earlier draft had a `(zone ...)` block with no
`filled_polygon` data at all, so opening the file fresh (or running DRC
without that flag) showed zero actual GND copper -- explaining a batch of
phantom "unconnected" ratsnest warnings that had nothing to do with the
routing itself.

Fixed by computing the fill directly (Shapely: zone boundary ∩ board
outline, minus every foreign-net pad/trace/via buffered by clearance) and
writing real `filled_polygon` data into both zone blocks. First attempt
at this had its own bug: a polygon with a hole (the board's real cutout,
see above) was emitted as two separate additive `filled_polygon` entries
-- an outer shape plus the hole's own boundary as a *second* filled shape
-- which paints solid copper right back over the hole instead of
excluding it. Fixed with the standard "keyhole" technique: cut a thin
slit from the hole out to the exterior boundary so the whole island is
one simple, hole-free ring.

## Routing bugs found during review

- A via for one net (VBAT_4V2) physically overlapped a crossing trace
  from a different net (SPI_CLK) on the same layer -- a real short. Root
  cause: two separately-computed net paths shared the same physical trunk
  (one was a redundant subset of the other); dropping the redundant path
  removed the overlapping via entirely.
- Missing vias at real F.Cu/B.Cu layer transitions, caused by not
  correctly tracking which endpoint was the true shared connection point
  between consecutive segments in a reversed-order path reconstruction
  (Dijkstra's predecessor-walk produces the path back-to-front).
- Explicit point-to-point GND traces were redundant once both layers had
  a real filled pour (every GND pad is F.Cu-only, so the F.Cu pour alone
  already connects them) -- removed entirely in favor of 1-2 stitching
  vias tying the two pours together, and verified those vias actually
  land inside real board copper (an earlier stitching via placement
  landed 0.93mm outside the true board outline -- "floating in space").

## Verification

`kicad-cli pcb drc` (KiCad 10.0.4, not the system default 9.0.8 --
version mismatch between the two installed CLIs caused a "Failed to load
board" red herring early on) confirms 0 unconnected items and 0
shorting_items on the current revision. Remaining ~46 violations are
DFM-grade (clearances, drill sizing, silk overlap) -- normal manual
cleanup, not electrical faults.
