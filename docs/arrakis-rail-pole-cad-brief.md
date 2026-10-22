# Arrakis Rail — CAD brief for the pole and foot ("Thumper", pyramid foot)

Prompt for an external CAD / 3D-modelling agent. Paste everything below the line.

> Note: this is the Thumper pole variant. The proposed design (telescopic Seeker pole on the Shield Wall foot) is specified in `arrakis-rail-stl-brief.md`.
Design reference: Claude Design canvas "Dune LED Strip Holder", sheet 09 (Rev E, 2026-10-22). The lamp head is a separate brief (`arrakis-rail-cad-brief.md`); this brief is self-contained for the pole.

---

## Task

Model the pole of a desk task lamp called **Arrakis Rail**. The pole is also the foot: one hollow, 5 mm walled body. Its teardrop ("Dune") section runs straight from a closed spire at the top down to 160 mm above the table, where it lofts into a three-sided pyramid with rounded corners that stands on three pads. There is no stem. The pole carries a sliding clamp-on lamp head and houses the power switch and the power inlet. Split it into parts that print without supports, model the joints, and deliver STEP + STL for every part plus one assembled STEP. Work in millimetres. Keep the fixed numbers unless a check in the last section fails; then change the minimum and report what you changed.

## Coordinate frame

- Origin: on the pole's axis, at the **table surface**.
- **+Z** up. **+X** toward the lamp arm (the user). **+Y** completes a right-handed frame.
- The lamp head clamps around the pole with its collar bottom anywhere between Z = 326 and Z = 486; its arm points to +X and its cable enters the pole through a slot on the pole's +X face.

## The Dune profile

Every horizontal section of the pole above the foot is the same teardrop. Define `DUNE` in the XY plane:

- A circle of radius 35 centred on the axis, kept for X ≥ 0 (the round side faces +X).
- Two arcs of radius 70: one centred at (0, −35) from (0, +35) to the nose; one centred at (0, +35) from (0, −35) to the nose. They meet on the −X axis at X = −60.6 with a 60° included angle; round the nose R6.
- Size: 70 wide (Y) × 95.6 long (X). The circle side is where the head clamps; the crest points away from the user.

`DUNE_IN` = `DUNE` offset inward by 5 (the inside of the 5 mm wall).

## The foot outline

`FOOT` in the XY plane at Z = 2: a triangle with vertices **(185, 0)**, **(−70, +130)**, **(−70, −130)**, every corner rounded **R25**. It is 255 long (X) and 260 wide (Y). The vertex at (185, 0) points toward the user, under the arm; the flat side faces the wall.

## The body, bottom to top (one solid before splitting)

1. **Pyramid foot** — loft from `FOOT` at Z = 2 to `DUNE` at Z = 160, guided so that the (185, 0) corner meets the pole's round face at (35, 0), the two rear corners meet the pole's sides at (0, ±35), and the midpoint of the rear side meets the crest at (−60.6, 0). Fillet the three lateral edges (corner to pole) **R20** so the pyramid has curved edges. Shell 5 mm, open at Z = 2. Face angles from horizontal: front about 47°, sides about 59°, rear about 87°; nothing may be shallower than 45°.
2. **Pole** — `DUNE` extruded Z = 160 … 500, 5 mm wall. **Cable slot**: 8 wide (Y ±4), ends R4, cut through the +X wall from Z = 330 to Z = 490. The face between Z = 320 and 500 is where the head's split collar (bore `DUNE` + 0.1) clamps; keep it smooth, no grooves or text.
3. **Spire** — loft `DUNE` at Z = 500 to a 4 mm wide ridge along the X axis from X = −40 to X = +20 at Z = 560; the ridge is solid, every face ≥ 60°. The top is closed.
4. **Pads** — inside each rounded corner of the foot add a boss 24 × 24 × 8 (Z = 2 … 10) merged into the wall, with a Ø13 × 2 recess from below centred at **(143, 0)**, **(−52, +100)** and **(−52, −100)** for TPU pads Ø13 × 4. These sit inside the R25 corner roundings (the front corner's outline only reaches X = 155), at least 5 mm of wall around each recess. The pads touch the table at Z = 0; the rim floats at Z = 2.
5. **Button hole** — on the front face (the one that rises from the (185, 0) corner), centred at Y = 0, **Z = 70**, which falls at X ≈ 120. Ø**19.2** through the wall, axis normal to the face (about 43° above horizontal, pointing at the user). Flatten a Ø26 landing around the hole on the outside, 0.5 deep, so the bezel and nut seat flat. Clear space inside: a Ø24 cylinder 35 long behind the hole. (A 19 mm anti-vandal push button; latching or momentary as supplied.)
6. **XT60E-M cut-out** — on the rear face, centred at Y = 0, **Z = 20**, which falls at X ≈ −69: a rectangular opening **16.0 × 8.6**, R1 corners, long side along Y, axis along −X; two screw holes on the Y axis at **21.4** centres, Ø**2.2** for M2.5 screws threading straight into the 5 mm wall. Recess the connector's 27.1 × 9.1 flange 1 mm into the outside face. Keep 25 mm clear behind the wall for solder tails and heat-shrink. **Verify 21.4 and the screw size against the AMASS XT60E-M drawing or a physical part before cutting; retailers disagree (M2.5 vs M3).**
7. Finishing — 1 × 45° chamfer on the rim; break every other edge 0.5. No decorative grooves on the pole (the head's collar carries them).

## Split into printable parts

Cut the body at **Z = 200** and **Z = 400**. The profile is asymmetric, so every joint self-indexes; the slot must line up across the Z = 400 joint.

- **P1, foot segment** — pyramid foot plus pole to Z = 200, with pads bosses, button hole and XT60 cut-out. Prints standing on its open rim; every face leans inward, so no supports. Footprint 255 × 260: needs a **260 × 260 bed**. For smaller beds expose `FOOT_SPLIT = halves`: cut P1 on the XZ plane (Y = 0) into two mirrored halves, each with a 5 × 5 tongue on one cut face and a matching groove (0.1 clearance) on the other, running the full height of the cut; halves glue together, then the pole joint above is as below.
- **P2, middle segment** — Z = 200 … 400: pole with the slot from Z = 330. Prints standing on its lower cut face.
- **P3, top segment** — Z = 400 … 560: pole with the slot to Z = 490, spire. Prints standing on its lower cut face; the ridge is the last layers.
- **Joints** — the upper part of each joint carries a **spigot**: a 20 mm long ring below its cut face whose outer profile is `DUNE_IN` minus 0.2 all round and whose wall is 2.5 thick. Add a 45° chamfer where the spigot steps out to the full wall so the overhang stays printable. The spigot at Z = 400 is notched 8 wide on the +X side so the slot stays open through the joint. Joints are glued (CA or epoxy); add one Ø3 cross hole per joint on the −X side for an optional M3 pin.
- **P4** — TPU pads Ø13 × 4 (×3), 100 % infill, 95A.
- **Optional P5** — a 2 mm bottom plate that snaps into the foot rim (0.15 interference ribs), with a 20 mm hand hole; off by default.

## Interfaces to respect

- The head's collar: bore `DUNE` + 0.1, 40 tall, collar bottom between Z = 326 and Z = 486; its cable passage is 4 above its bottom and must always meet the slot.
- The head's cable (Ø4) runs: slot → inside the pole → foot cavity → button (in the + lead) → XT60E-M. Keep a Ø6 clear path all the way; no internal ribs across the bore.
- The head as a reference body for the assembly: a 256 × 84 × 14 blade on a Ø84 × 40 collar, 0.30 kg, centre of mass at X = 80 above the collar bottom; place it with the collar bottom at Z = 400.

## Printing

- 0.2 mm layers, 5 mm walls as modelled (slice with enough perimeters to make the wall solid, or 3 perimeters plus 40 % gyroid inside the 5 mm; state which you assumed).
- All parts stand on their cut faces; the foot stands on its rim. No supports: every overhang ≤ 45° from vertical except bridges ≤ 8 mm.
- Materials: PLA, sand or bronze to match the head; TPU for the pads.

## Checks to run before delivering

1. Wall thickness ≥ 5 everywhere (report any spot under 5, including the pyramid-to-pole transition, the filleted edges and the spigot steps).
2. Overhangs: no face steeper than 45° overhang in the stated orientations; list the worst face of each part and the measured front, side and rear face angles of the foot.
3. Slot: continuous 8 wide from Z = 330 to 490 across the P2/P3 joint, spigot notch included.
4. Cable path: a Ø6 sphere passes from the slot to the XT60 opening.
5. Button and XT60 clearances inside the foot as stated; the button's Ø24 nut clears the wall and the pad bosses.
6. Stability: support triangle with vertices at the three pad centres (143, 0), (−52, ±100). Compute the whole-lamp centre of mass from the model masses plus the head (0.30 kg at X = 80, Z = 400). Required: ≥ 50 mm margin from the centre of mass to every edge of the triangle; report the margins and the tip-over force at the blade tip (X = 256, Z = 400, downward) and sideways at the head (Z = 400). Expected: about 1.5 kg total, centre 21 mm toward +X, margins about 56 mm (front edges) and 73 mm (rear), about 1.6 kg and 0.21 kg.
7. Bed fit: report the footprint and height of each part for a 260 bed and, with `FOOT_SPLIT = halves`, for a 200 bed.
8. Mass per part at 5 mm walls; expect about 1.2 kg for P1 + P2 + P3 (foot about 630 g). Also report the total if `WALL` were 3.

## Deliverables

- `pole_p1_foot.step/.stl` (or `pole_p1_foot_left/right` when split), `pole_p2_middle.step/.stl`, `pole_p3_spire.step/.stl`, `toe_pad.step/.stl`, `bottom_plate.step/.stl` (optional), `arrakis_rail_pole_assembly.step` (all pole parts, pads, and the head reference at Z = 400).
- A short report with the check results, the foot face angles, the stability margins, the per-part masses and bed footprints, and any dimension you changed and why.
- Parameters to expose: `WALL` (5), `FOOT_SPLIT` (one | halves), `BED` (260), `HEIGHT_TOTAL` (560), `FOOT_HEIGHT` (160), `FOOT_VERTICES` ((185, 0), (−70, ±130)), `CORNER_R` (25), `EDGE_R` (20), `SLOT_Z` (330 … 490), `BUTTON_D` (19.2), `BUTTON_Z` (70), `XT60_HOLE_PITCH` (21.4), `XT60_SCREW_HOLE` (2.2), `PAD_D` (13).
