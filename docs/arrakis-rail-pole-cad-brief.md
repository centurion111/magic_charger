# Arrakis Rail — CAD brief for the pole and foot ("Thumper")

Prompt for an external CAD / 3D-modelling agent. Paste everything below the line.
Design reference: Claude Design canvas "Dune LED Strip Holder", sheet 09 (Rev C, 2026-10-21). The lamp head is a separate brief (`arrakis-rail-cad-brief.md`); this brief is self-contained for the pole.

---

## Task

Model the pole of a desk task lamp called **Arrakis Rail**. The pole is also the foot: one hollow, 5 mm walled body that stands on a flat table on three fins, carries a sliding clamp-on lamp head, and houses the power switch and the power inlet. Split it into parts that print on a 200 × 200 × 200 mm bed with no supports, model the joints, and deliver STEP + STL for every part plus one assembled STEP. Work in millimetres. Keep the fixed numbers unless a check in the last section fails; then change the minimum and report what you changed.

## Coordinate frame

- Origin: on the pole's axis, at the **table surface**.
- **+Z** up. **+X** toward the lamp arm (the user). **+Y** completes a right-handed frame.
- The lamp head clamps around the pole between Z = 330 and Z = 490 and its arm points to +X; the head's cable enters the pole through a slot on the pole's +X face.

## The Dune profile

Every horizontal section of the pole is the same teardrop, scaled. Define `DUNE(s)` in the XY plane, scale `s` about the axis:

- A circle of radius 35·s centred on the axis, kept for X ≥ 0 (the round side faces +X).
- Two arcs of radius 70·s: one centred at (0, −35·s) from (0, +35·s) to the nose; one centred at (0, +35·s) from (0, −35·s) to the nose. They meet on the −X axis at X = −60.6·s with a 60° included angle; round the nose R 6·s.
- Size at s = 1: 70 wide (Y) × 95.6 long (X). The circle side is where the head clamps; the crest points away from the user.

`DUNE_IN(s)` = `DUNE(s)` offset inward by 5 (the inside of a 5 mm wall).

## The body, bottom to top (one solid before splitting)

1. **Hub** — `DUNE(1.3)` (91 × 124) extruded Z = 2 … 32, then cut by a flat back plane at **X = −62** (the flat is 42 wide). Shell: 5 mm walls, open at Z = 2. The hub rim floats 2 mm above the table; the fins' toes carry the lamp.
2. **Roof** — loft from the hub outline at Z = 32 to `DUNE(0.5)` at Z = 80, 5 mm shell. The round side of this loft is about 60° from horizontal, the crest side about 45°; nothing may be shallower than 45°.
3. **Stem** — `DUNE(0.5)` (35 × 48; inside about 25 × 38) extruded Z = 80 … 290, 5 mm wall.
4. **Flare** — loft `DUNE(0.5)` at Z = 290 to `DUNE(1.0)` at Z = 320, 5 mm wall (45° to 60° faces).
5. **Clamp zone** — `DUNE(1.0)` extruded Z = 320 … 500, 5 mm wall. **Cable slot**: 8 wide (Y ±4), ends R4, cut through the +X wall from Z = 330 to Z = 490. This face is where the head's split collar (bore `DUNE(1.0)` + 0.1) clamps; keep it smooth, no grooves or text.
6. **Spire** — loft `DUNE(1.0)` at Z = 500 to a 4 mm wide ridge along the X axis from X = −40 to X = +20 at Z = 560; the ridge is solid, every face ≥ 60°. The top is closed.
7. **Fins** ×3, 5 thick, vertical plates, merged into the hub wall. Bottom edge at Z = 2; top edge a straight line from 30 tall at the hub to 8 tall at the tip; leading (top) edge chamfered 1.5 × 45°:
   - F1 along +X from the hub to a tip at (190, 0).
   - F2 and F3 on rays through the axis to (−40, +150) and (−40, −150).
   Each tip ends in a **toe**: a Ø16 × 8 cylinder (Z = 2 … 10) merged into the fin, with a Ø13 × 2 recess from below for a TPU pad Ø13 × 4. The three pads are the only contact with the table.
8. **Button hole** — Ø**19.2** through the roof's +X facet, axis normal to that facet, centred at X = 31.5, Y = 0, Z = 56. Flatten a Ø26 landing around the hole on the outside (0.5 deep) so the button's bezel and nut seat flat. Clear space inside: a Ø24 cylinder 35 long behind the hole, nothing in its way. (The part is a 19 mm anti-vandal push button; latching or momentary as supplied.)
9. **XT60E-M cut-out** — on the flat back face X = −62, centred at Y = 0, Z = 17: a rectangular opening **16.0 × 8.6**, R1 corners, long side along Y; two screw holes on the Y axis at **21.4** centres, Ø**2.2** for M2.5 screws threading straight into the 5 mm wall. Recess the connector's 27.1 × 9.1 flange 1 mm into the outside face. Keep 25 mm clear behind the wall for solder tails and heat-shrink. **Verify 21.4 and the screw size against the AMASS XT60E-M drawing or a physical part before cutting; retailers disagree (M2.5 vs M3).**
10. Finishing — 1 × 45° chamfer on the hub rim and fin bottoms; break every other edge 0.5. No decorative grooves on the pole (the head's collar carries them).

## Split into printable parts

Cut the body at **Z = 200** and **Z = 400**. The profile is asymmetric, so every joint self-indexes; the slot must line up across the Z = 400 joint.

- **Part P1, segment 1** — hub, roof, stem to Z = 200, with the fin roots or fin sockets (below), the button hole, the XT60 cut-out. Prints standing on its open rim. Footprint 91 × 124 without fins.
- **Part P2, segment 2** — Z = 200 … 400: stem, flare, lower clamp zone with the slot from Z = 330. Prints standing on its lower cut face.
- **Part P3, segment 3** — Z = 400 … 560: upper clamp zone with the slot to Z = 490, spire. Prints standing on its lower cut face; the spire's ridge is the last layers.
- **Joints** — the upper part of each joint carries a **spigot**: a 20 mm long ring below its cut face whose outer profile is `DUNE_IN(s)` of the lower part minus 0.2 all round and whose wall is 2.5 thick. Add a 45° chamfer where the spigot steps out to the full wall so the overhang stays printable. The spigot at Z = 400 is notched 8 wide on the +X side so the slot stays open through the joint. Joints are glued (CA or epoxy); add one Ø3 cross hole per joint for an optional M3 pin, on the −X side.
- **Fins** — expose parameter `FINS`:
  - `integral`: fins modelled as part of P1 (needs a 300 × 300 bed; the three toes then define the bed footprint 230 × 300).
  - `dovetail` (default): fins are parts **P4 (front)** and **P5 (rear, ×2, identical)**, printed lying flat. Each fin carries a dovetail tenon on its root edge: 6 deep, 12 wide at the wide end, 8° flanks, running the full 30 mm root height. P1 gets matching mortises in local bosses: thicken the hub wall to 10 mm over a 24 mm wide strip at each fin root (inside the hub) so the 6 mm mortise does not break through. Tenons slide in from below; 0.1 clearance per side, glued.
- **Part P6** — TPU toe pads Ø13 × 4 (×3), 100 % infill, 95A.
- **Optional Part P7** — a 2 mm bottom plate that snaps into the hub rim (0.15 interference ribs), with a 20 mm hand hole; off by default.

## Interfaces to respect

- The head's collar: bore `DUNE(1.0)` + 0.1, 40 tall, can sit anywhere with its bottom between Z = 326 and Z = 486; its cable passage is 4 above its bottom and must always meet the slot.
- The head's cable (Ø4) runs: slot → inside the clamp zone → stem bore (≥ 25 × 38 clear) → hub cavity → button (in the + lead) → XT60E-M. Keep a Ø6 clear path all the way; no internal ribs across the stem bore.
- The head as a reference body for the assembly: a 256 × 84 × 14 blade on a Ø84 × 40 collar, 0.30 kg, centre of mass at X = 80 above the collar bottom; place it at Z = 400 in the assembly.

## Printing

- 0.2 mm layers, 5 mm walls as modelled (slice with enough perimeters to make the wall solid, or 3 perimeters plus 40 % gyroid inside the 5 mm; state which you assumed).
- All parts stand on their cut faces; the hub stands on its rim. No supports anywhere: every overhang must be ≤ 45° from vertical except bridges ≤ 8 mm.
- Materials: PLA, sand or bronze to match the head; TPU for the pads.

## Checks to run before delivering

1. Wall thickness ≥ 5 everywhere (report any spot under 5, including the roof-to-stem corner and the spigot steps).
2. Overhangs: no face steeper than 45° overhang in the stated orientations; list the worst face of each part.
3. Slot: continuous 8 wide from Z = 330 to 490 across the P2/P3 joint, spigot notch included.
4. Cable path: a Ø6 sphere passes from the slot to the XT60 opening.
5. Button and XT60 clearances inside the hub as stated; the button's Ø24 nut clears the fin bosses and the roof.
6. Stability: support triangle with vertices at the three pad centres (190, 0), (−40, ±150). Compute the whole-lamp centre of mass from the model masses plus the head (0.30 kg at X = 80, Z = 400). Required: ≥ 50 mm margin from the centre of mass to every edge of the triangle; report the margins and the tip-over force at the blade tip (X = 256, Z = 400, downward) and sideways at the head (Z = 400). Expected: about 1.0 kg total, centre 22 mm toward +X, 2.4 kg and 0.24 kg.
7. Bed fit: every part within 200 × 200 × 200 when `FINS = dovetail`; report the footprint and height of each.
8. Mass per part at 5 mm walls; expect about 0.7 kg for P1 + P2 + P3. Also report the saving if `WALL` were 3.

## Deliverables

- `pole_p1_hub.step/.stl`, `pole_p2_stem.step/.stl`, `pole_p3_spire.step/.stl`, `fin_front.step/.stl`, `fin_rear.step/.stl`, `toe_pad.step/.stl`, `bottom_plate.step/.stl` (optional), `arrakis_rail_pole_assembly.step` (all pole parts, pads, and the head reference at Z = 400).
- A short report with the check results, the roof facet angles, the stability margins, the per-part masses and bed footprints, and any dimension you changed and why.
- Parameters to expose: `WALL` (5), `FINS` (dovetail | integral), `BED` (200), `HEIGHT_TOTAL` (560), `SLOT_Z` (330 … 490), `BUTTON_D` (19.2), `XT60_HOLE_PITCH` (21.4), `XT60_SCREW_HOLE` (2.2), `FIN_REACH_FRONT` (190), `FIN_REACH_REAR` (−40, ±150), `PAD_D` (13).
