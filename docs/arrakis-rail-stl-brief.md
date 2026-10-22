# Arrakis Rail — CAD brief for all printed parts (Shield Wall foot, Seeker pole, Hawk head)

Prompt for an external CAD / 3D-modelling agent. Paste everything below the line. It is self-contained and supersedes `arrakis-rail-cad-brief.md` and `arrakis-rail-pole-cad-brief.md` for the proposed design; the canvas "Dune LED Strip Holder" sheets 10 to 13 (Rev A, 2026-10-22) are the drawings behind it.

---

## Task

Model every printed part of a desk task lamp called **Arrakis Rail** and deliver a printable binary STL plus a STEP for each part, and one assembled STEP. The lamp is: a flat LED head (the "Hawk" blade, four 8 × 150 mm strips facing down) clamped on a telescopic pole (two tubes of a teardrop "Dune" section, locked by a large knob), standing in the cleft of a bisected pyramid foot ("Shield Wall"). Everything is FDM printed in PLA except the pads and a grommet in TPU. Work in millimetres. Keep the fixed numbers unless a check in the last section fails; then change the minimum and report what you changed. Where a number is given as a parameter in brackets, build it parametrically.

## Coordinate frames

- **Lamp frame**: origin on the pole axis at the table surface. **+Z** up. **+X** toward the user, the direction the lamp arm points. **+Y** completes a right-handed frame (the user's right when facing the lamp).
- **Tube frame** (parts P3, P4): z' measured from the bottom end of the upper tube; same X and Y. In the lamp the tube bottom sits at Z = 90 (fully down) to Z = 220 (fully up).
- **Head frame** (parts P11 to P14): origin on the pole axis in the plane of the deck's underside, which is also the collar's bottom face. +Z up, +X along the arm. The collar bottom sits at z' = 256 to 316 on the tube.

## Profiles

**DUNE(d)**, a teardrop in the XY plane offset outward by d (d may be negative):
- a circle of radius 35 + d centred on the axis, kept for X ≥ 0 (the round side faces the user);
- two arcs of radius 70 + d, one centred at (0, −35) running from (0, 35 + d) to the nose, one centred at (0, +35) running from (0, −35 − d) to the nose; they meet on the −X axis at 60°; the nose is rounded R(6 + d). Note the arc centres do not move with d.
- DUNE(0) is 70 wide (Y) and 95.6 long (X), nose at X = −60.6.
- Used: DUNE(0) tube outside · DUNE(−5) tube bore · DUNE(+0.5) sleeve bore · DUNE(+5.5) sleeve outside · DUNE(+0.1) head collar bore · DUNE(+7) cap outside · DUNE(−8) stop ring opening.

**FOOT**, the pyramid base in the XY plane at Z = 2: the rectangle X = −70 … 165, Y = ±130 with every corner rounded R25.

**PYRAMID**: the solid with base FOOT at Z = 2 and apex at (0, 0, 175) (a loft from the base outline to the point, or an extrude-to-point). Its faces: front 46.4° from horizontal, sides 53.1°, back 68.0°. The corner roundings become conical patches.

## Parts

### P1 — Foot with the lower sleeve (sand PLA)

1. **Halves**: `PYRAMID − SlotBox − SleeveSolid`, where SlotBox removes everything with |Y| < 25 (full X and Z) and SleeveSolid is DUNE(+5.5) extruded Z = 2 … 330. Shell the result **5 mm** with the Z = 2 face removed. Each half is then a hollow shell: outer faces, a vertical cut face at |Y| = 25 (5 thick, inner face at |Y| = 30), and an inner wall wrapping the sleeve where the sleeve passes through it. The halves top out at Z = 141.7 on the slot edges, over X = −13.5 … 31.7.
2. **Sleeve**: tube DUNE(+5.5) outside, DUNE(+0.5) bore, Z = 2 … 170, unioned with the halves. Above 170 the sleeve continues as part P2 (lap joint below).
3. **Lap joint at 170**: keep the inner half of the sleeve wall, DUNE(+0.5) … DUNE(+3.0), from Z = 170 up to Z = 190 as a spigot. Chamfer its top outer edge 0.5 × 45°.
4. **Stop ring** inside the bore: a ledge from DUNE(+0.5) in to DUNE(−8), Z = 90 … 94, with a 0.5 × 45° chamfer on its top inner edge. The upper tube lands on it.
5. **Key groove** in the bore wall on the +Y side: 9 wide (X = ±4.5), 2 deep radially, from Z = 96 to Z = 236, ends R4.5, closed at both ends. It runs across the spigot (P1) and continues in P2.
6. **Wiring holes**: Ø10 along Y at X = 0, Z = 30, through the sleeve wall and the half's inner wall on both sides, into the halves' cavities. Chamfer 1 × 45°.
7. **Button hole**, right half (Y > 0), on the front face: the face plane is Z = 175 − 1.0485 X. Centre **(100, 50, 70.2)**, axis along the face normal **(0.7235, 0, 0.6901)**. Ø**19.2** through; a Ø26 flat landing 0.5 deep on the outside so the bezel and nut seat flat; keep a Ø24 cylinder 35 long clear behind the hole along −normal (19 mm anti-vandal push button).
8. **XT60E-M cut-out**, left half (Y < 0), on the back face: the face plane is Z = 2 + 2.4714 (X + 70). Centre **(−62.7, −60, 20)**, axis along the face normal **(−0.927, 0, 0.375)**. Rectangular opening **16.0 × 8.6**, R1 corners, long side along Y; two screw holes Ø**2.2** on the line through the centre along Y at **21.4** centres (M2.5 screws into the 5 mm wall); recess the 27.1 × 9.1 flange 1 mm into the face. Keep 25 mm clear behind the wall. **Verify 21.4 and the screw size against an AMASS XT60E-M drawing or a physical part before cutting.**
9. **Pad bosses**: inside each of the four base corners a 24 × 24 × 8 boss (Z = 2 … 10) merged with the walls, with a Ø13 × 2 recess from below centred at **(140, ±105)** and **(−45, ±105)**. Pads P9 touch the table at Z = 0; the rim floats at Z = 2.
10. **Floor-plate lips** (for the optional P10): a 2 × 2 rebate along the inside of each half's rim at Z = 2 … 4.
11. Finishing: 1 × 45° chamfer on the rim; break all other outside edges 0.5. No text or grooves on the faces.

Expose `FOOT_SPLIT` (one | halves). With `halves`: cut P1 on the plane Y = +25 … the right half (Y ≥ 25, with its wrap around the sleeve's bulge) becomes **P1R** and the rest (left half with the whole sleeve and the sleeve's bulge into the right half) stays **P1**. Give the joint a 3 × 3 tongue on P1 along the sleeve's bulge and a matching groove (0.1 clearance) in P1R; the parts glue.

### P2 — Upper sleeve with the lock boss (sand PLA)

1. Tube DUNE(+5.5) outside, Z = 170 … 330; bore DUNE(+0.5) from Z = 190 to 330. From Z = 170 to 190 keep only the outer half of the wall, DUNE(+3.15) … DUNE(+5.5): it slides over P1's spigot with 0.15 radial clearance. Chamfer the bottom inner edge 0.5 × 45°.
2. Key groove as in P1 item 5, Z = 190 … 236 (its closed top end is the tube's pull-out stop).
3. **Key port**: Ø10 along Y through the +Y wall at X = 0, **Z = 232**, from the outside face down to the groove bottom. P8 plugs it.
4. **Lock boss** on the crest nose at **Z = 290**: a prism of section DUNE scaled 32/70 (32 wide, 43.7 tall), crest **upward**, axis along X, running from the nose faces out to **X = −94**; merge it into the nose. Through its axis: a printed thread **M20 × 2.5** from X = −94 to X = −76 (18 engaged), then a plain bore Ø**13** from X = −76 through the nose wall into the sleeve bore. Model the thread (0.3 radial clearance to the knob); a 45° lead-in chamfer at the face.
5. **Fin** under the boss so it prints without support: 5 thick (Y = ±2.5), from the boss underside at (X −94, Z 274) down at 45° to the nose at Z = 246.
6. Rim at 330: 1 × 45° outside chamfer. The outside between Z = 190 and 330 stays smooth.

### P3 — Upper tube, lower half (sand PLA)

Tube frame. Outside DUNE(0) from z' = 0 to 210; bore DUNE(−5) from z' = 0 to 190. From z' = 190 to 210 keep only the outer half of the wall, DUNE(−2.5) … DUNE(0): P4's inner half fits inside it. Bottom end open with a 1 × 45° outside chamfer. **Key pocket**: Ø8.2 × 3 deep, axis along +Y, on the +Y face at X = 0, **z' = 12**. Chamfer the top inner edge 0.5 × 45°.

### P4 — Upper tube, top half with the slot and spire (sand PLA)

Tube frame. Bore DUNE(−5) from z' = 190; outside DUNE(0) from z' = 210 to 360. From z' = 190 to 210 keep only the inner half of the wall, DUNE(−5) … DUNE(−2.65) (0.15 radial clearance into P3). **Spire**: from z' = 360 loft DUNE(0) to a 4 mm wide ridge along the X axis from X = −40 to X = +20 at z' = 380; close the top; every face ≥ 60°. **Cable slot**: 8 wide (Y = ±4), ends R4, through the +X wall from **z' = 260 to 320**. The outside from 210 up to 360 is the head's clamping surface and the sliding surface: no features.

### P5 — Lock knob (bronze PLA, spice inlay)

A trilobe disc 16 thick: outline = circle R24 with three scallops R10 whose centres lie on a circle R26 at 60°, 180° and 300°; the lobes are at 0°, 120°, 240°; blend the scallop-to-lobe transitions R3; round the outer face edge R2. On its inner face a shank: printed external thread **M20 × 2.5**, 19 long (X toward the pole), ending in a Ø8 × 4 pin. On its outer face a Ø30 × 1 recess for the inlay P5a (a Ø29.8 × 1.2 disc in spice PLA, glued, or printed in place with a colour change). The knob's total height on the bed is 16 + 19 + 4.

### P6 — Shoe (bronze PLA)

Cylinder Ø12.8 × 12, axis along X. Front face: a 60° V-notch 3 deep across the full diameter, V axis along Z, so it cradles the tube's crest. Back face: a Ø8.2 × 4 recess for the knob's pin. Chamfer 0.5.

### P7 — Key (spice PLA)

Cylinder Ø8.0 × 5.0, one end chamfered 0.5. Glued 3 deep into P3's pocket, 2 proud; it runs in the 9 × 2 groove with 0.5 clearance to the groove bottom (0.5 bore gap + 2 groove − 2 proud).

### P8 — Port plug (spice PLA)

Cylinder Ø10.1 × 3.0, 0.5 chamfer on the outer end: a light press fit in the Ø10 port, outer face flush with the sleeve, inner face flush with the groove bottom.

### P9 — Pads (TPU 95A), ×4

Ø13 × 4, 100 % infill, 2 mm of them in the recess.

### P10 — Floor plates (sand PLA), ×2, optional

For each half: a 2 mm plate matching the half's base outline inset 5.2 (so it drops into the rebate), with a 20 mm hand hole and four 0.3-high crush ribs on its edges for a snap fit. Off by default (`FLOOR_PLATES = false`).

### P11 — Deck half (sand PLA) · head frame

Union of the following, then the cuts.

1. **Blade slab**: plan polygon extruded Z = 0 … 14: `(0, +42) → (196, +42) → (256, −14) → (200, −42) → (0, −42)` and back along X = 0.
2. **Top chamfer**: a distance-distance chamfer on every top perimeter edge except the root edge at X = 0, **20 along the top face, 6 down the sides**: a plateau at Z = 14 that is the outline inset 20 (its tip near (221.4, −9)), facets of about 17°, a vertical edge wall 8 tall.
3. **Half ring**: cylinder OD 84, Z = 0 … 40, centred on the axis, kept for X ≥ 0.
4. **Lugs ×2**: X = 0 … 7, Y = +42 … +56 and −56 … −42, Z = 0 … 40.
5. **Bolt holes ×4**, axis along X: Ø4.3 through the lugs at Y = ±49, Z = 12 and 30; no counterbore. Model them as teardrops (point up) for the print, or note "drill to Ø4.3".
6. **Bore**: remove DUNE(+0.1) through the whole part (Z = 0 … 40); the deck half carries the round side.
7. **Split-face setback**: remove 0.5 from the part at the split plane so its X = 0 faces become X = +0.5 (ring wall and lugs). With the cap this gives a 1 mm clamp gap at nominal size.
8. **LED channels ×4** in the underside, depth 2.6: X = 56 … 208, width 8.4, centrelines Y = −21, −7, +7, +21; mouth edges chamfered 0.8 × 45°.
9. **Junction trough** in the underside: X = 46 … 56, Y = −28 … +28, depth 8, inside corners R1; around its mouth a 1.5-deep rebate X = 45.8 … 56.2, Y = −28.2 … +28.2 for the cover P13.
10. **Zip-tie post**: Ø3 pin at (51, −20) from the trough ceiling (Z = 8) down to Z = 2.
11. **Cable passage**: Ø6 along −X at Y = 0, Z = 4, from the trough's back wall (X = 46) into the bore (breaking through the ring at X ≈ 35.1); chamfer both ends 1 × 45°.
12. **Grooves** on the ring's exposed outside: three, 1 wide × 1 deep, at Z = 20, 26, 32.
13. Chamfer the ring's top outer edge 1 × 45°; break lug edges 0.5.

### P12 — Cap (bronze PLA) · head frame

1. **Sleeve**: DUNE(+7) extruded Z = 0 … 40, kept for X ≤ 0 only (R42 on the circle side, R77 arcs around the crest, nose wall 8 thick at Y = 0).
2. **Lugs ×2**: X = −10 … 0, Y = +42 … +56 and −56 … −42, Z = 0 … 40.
3. **Heat-set insert pockets ×4** from the split face along −X: Ø5.6, depth 8, at Y = ±49, Z = 12 and 30 (M4 inserts).
4. **Bore**: remove DUNE(+0.1), crest half.
5. **Split-face setback** 0.5 (faces become X = −0.5).
6. Grooves at Z = 20, 26, 32 continuing P11's; nose edges rounded; 1 × 45° on the top outer edge.

### P13 — Trough cover (sand PLA)

Plate 10.0 × 56.0 × 1.5 for P11's rebate, flush with the underside; four crush ribs 0.3 high × 0.6 wide × 6 long on the long sides (0.15 interference per side); a 2 mm fingernail notch in one short edge.

### P14 — Grommet (TPU), optional

Tube Ø6.0 outside, Ø4.0 bore, 11 long, 1 mm lip at one end, for the cable passage.

### Hardware (not modelled; list in the BOM)

4 × M4 × 16 socket-head cap screws · 4 × M4 heat-set inserts Ø5.6 × 8 · 2 × M2.5 × 8 screws (XT60) · 1 × 19 mm anti-vandal push button, latching · 1 × AMASS XT60E-M panel inlet · 4 × LED strip 8 × 150 mm, 12 V, adhesive, 2.2 thick · twin cable 2 × 20 AWG Ø4, 1.2 m · 1 × 2.5 mm zip tie.

## Assembly relationships to encode in the assembled STEP

- P2 on P1 at the Z = 170 lap joint; P4 on P3 at the z' = 190/210 lap joint; the tube (P3 + P4) in the sleeve bore with its bottom at Z = 90 on the stop ring for the "down" configuration (also place a "up" configuration at Z = 220 if your tool allows).
- P7 in P3's pocket, in the groove; P8 in the port; P6 in the boss bore with its V against the tube's crest; P5 threaded in, pin in P6's recess, 1 mm of thread showing (tightened position).
- P9 in the four recesses; P10 in the rebates (if on).
- Head: P11 and P12 around the tube with a 1 mm gap at the split plane, collar bottom at z' = 286 (Z = 376 down, 506 up); screws through P11 into P12's inserts; P13 in P11's rebate; P14 in the passage. The passage (Z = 4 above the collar bottom) meets the tube's slot.
- Cable: trough → passage → slot → down the tube → through the stop ring → Ø10 holes → button (right half) and XT60 (left half). Keep a Ø6 clear path all the way.

## Print orientations (drive the overhang checks)

- **P1**: standing on its rim at Z = 2 (all faces lean inward or are vertical; the spigot ring is the last layers). Needs a 260 × 260 bed, else `FOOT_SPLIT = halves`.
- **P2**: standing on its Z = 170 face (the 2.5 mm outer-half ring; use a brim). The boss prints on its fin.
- **P3**: standing on z' = 0. **P4**: standing on its z' = 190 face (inner-half ring, brim); the spire is the last layers.
- **P5**: inlay face down, shank up (thread vertical). **P6**: back face down. **P7, P8, P9, P10**: flat.
- **P11**: underside (Z = 0) on the bed; the channels, trough and rebate are first-layer recesses. **P12**: split face down. **P13, P14**: flat.
- No supports anywhere: every overhang ≤ 45° from vertical, bridges ≤ 8 mm.

## Checks to run before delivering

1. Wall thickness ≥ 5 everywhere on P1 … P4 (report any spot under 5, including the halves' inner walls around the sleeve, the spigots and the boss root), ≥ 3 on the head parts except the stated 8 mm edge wall and 7 mm ring; report anything under 2.
2. Overhangs per the orientations above; report the worst face of each part and the measured pyramid face angles.
3. Sliding fit: the tube passes through the sleeve bore with 0.5 radial clearance everywhere, including across both lap joints; the slot is 8 wide for its whole length; the key runs from Z = 96 to 236 and stops with the tube bottom at Z = 220; the port lines up with the key at that position.
4. Lock: the shoe's V contacts the tube's crest with the knob 1 mm from fully in; the boss does not break into the key groove; the knob clears the head collar at the lowest head position (collar bottom Z = 346 vs boss top Z = 317.7).
5. Cable path: a Ø6 sphere passes from the head's trough to the XT60 opening and to the button cavity.
6. Button and XT60 clearances inside the halves as stated; the button's Ø24 nut clears the slot wall and the side face.
7. Stability: support rectangle with vertices at the four pad centres (140, ±105), (−45, ±105). Compute the whole-lamp centre of mass from the model masses plus the head (0.30 kg at X = 80 above the collar bottom), tube fully up. Required: ≥ 50 mm margin from the centre of mass to every edge; report the margins and the tip-over forces at the blade tip (X = 256, downward) and sideways and backward at the head. Expected: about 2.4 kg total, centre about 15 mm toward +X, margins about 125 / 60 / 105 mm, about 2.5 kg, 0.45 kg and 0.26 kg.
8. Bed fit: footprint and height of each part for a 260 bed; with `FOOT_SPLIT = halves` also for a 220 bed. Expected heights: P1 190, P2 160, P3 210, P4 190.
9. Mass per part at 5 mm walls (expect about 1.2 kg for P1, 0.31 P2, 0.36 P3, 0.33 P4, 0.05 P5, head parts about 0.3 kg together) and the total if `WALL` were 3.
10. Each STL: closed, manifold, consistent normals, no self-intersections; exported with chordal deviation ≤ 0.05 mm and angular deviation ≤ 5°, in millimetres, one body per file.

## Deliverables

- `P01_foot_sleeve.stl/.step` (or `P01_foot_left.*` + `P01R_foot_right.*` when split), `P02_sleeve_top.*`, `P03_tube_lower.*`, `P04_tube_upper.*`, `P05_knob.*`, `P05a_inlay.*`, `P06_shoe.*`, `P07_key.*`, `P08_plug.*`, `P09_pad.*`, `P10_floor_left.*`, `P10_floor_right.*`, `P11_deck_half.*`, `P12_cap.*`, `P13_trough_cover.*`, `P14_grommet.*`, `arrakis_rail_assembly.step`.
- A short report: the check results, the plateau polygon you obtained from the blade chamfer, per-part masses and bed footprints, print orientation per part, and any dimension you changed and why.
- Parameters to expose: `WALL` (5), `FOOT_SPLIT` (one | halves), `FLOOR_PLATES` (false), `SLOT_W` (50), `APEX_Z` (175), `FOOT_X` (−70 … 165), `FOOT_Y` (±130), `CORNER_R` (25), `SLEEVE_TOP` (330), `TUBE_LEN` (380), `TRAVEL` (130), `SLIDE_CLEARANCE` (0.5), `LAP` (20), `LOCK_Z` (290), `THREAD` (M20 × 2.5), `BUTTON_D` (19.2), `XT60_HOLE_PITCH` (21.4), `XT60_SCREW_HOLE` (2.2), `PAD_D` (13), `STRIP_WIDTH` (8), `STRIP_LENGTH` (150), `HAND` (left | right: mirror the head about XZ for a right-handed blade).
