# Arrakis Rail — CAD brief for the lamp head

Prompt for an external CAD / 3D-modelling agent. Paste everything below the line.
The design drawings live in the Claude Design canvas "Dune LED Strip Holder" (sheets 01–08, Rev C, 2026-10-21).

---

## Task

Model the head of a task lamp called **Arrakis Rail** as printable parts and deliver STEP + STL for each part, plus one assembled STEP. The head holds four LED strips face-down over a table and clamps to a vertical, hollow, 3D-printed pole. The pole and the base are separate designs; you model the head's parts and, as a reference body only, a 200 mm stub of the pole with its cable slot so the fit can be checked.

Work in millimetres. Build everything parametrically where the brief gives a parameter in brackets, e.g. `[BORE_PROFILE]`. Keep the fixed numbers below exactly unless a check in the last section fails; then report the change, do not silently alter the design.

## Coordinate frame

- Origin: on the pole's axis, in the plane of the deck's underside (the light face).
- **+X**: along the arm, from the pole toward the blade tip.
- **+Z**: up (toward the ceiling). The deck's underside is at Z = 0, the strips face −Z.
- **+Y**: completes a right-handed frame. The blade tip is swept toward **−Y**. (A mirrored, other-handed head is simply the same model mirrored about the XZ plane.)
- The clamp's split plane is the YZ plane, X = 0. The "deck half" is X ≥ 0, the "cap" is X ≤ 0.

## Pole (reference body, not a deliverable part)

Profile "Dune", extruded along Z:

- A circle Ø70 centred on the axis for X ≥ 0 (the round half faces the arm).
- For X ≤ 0 the profile grows into a crest: two arcs of **R70**, one centred at (0, −35) running from (0, +35) to the nose, one centred at (0, +35) running from (0, −35) to the nose. They meet at (−60.6, 0) at 60°; round the nose **R6**. Overall 70 wide (Y) × 95.6 long (X).
- Hollow, wall 3.
- **Cable slot**: on the round face, centred on Y = 0 at X ≈ +35, width **8** (Y ±4), length **160** along Z with R2 ends. Place it so the slot spans Z = −60 … +100 in the head's frame when the collar sits mid-range. The head's cable passes through this slot into the hollow pole and hangs down to the base.
- Bore clearance for the head: profile offset by +0.1 all round (Ø70.2 on the circle).

## Parts to model

### Part 1 — Deck half (one print, sand PLA)

Union of the following, then the cuts.

1. **Blade slab.** Plan polygon in XY (Hawk outline), extruded from Z = 0 to Z = 14:
   `(0, +42) → (196, +42) → (256, −14) → (200, −42) → (0, −42)` and back to start along X = 0.
2. **Top chamfer (the "flat pyramid").** Chamfer every top perimeter edge of the slab except the root edge at X = 0 with a distance-distance chamfer: **20 along the top face, 6 down the side faces**. Result: a plateau at Z = 14 that is the plan outline inset by 20 (its tip lands near (221.4, −9)), facets at about 17°, and a vertical edge wall 8 tall around the blade.
3. **Half ring.** Cylinder OD **84** (R42), Z = 0 … 40, centred on the axis, kept only for X ≥ 0 (it merges with the slab's root; the ring stands 26 above the plateau like a turret).
4. **Lugs ×2.** Blocks X = 0 … 7, Y = +42 … +56 and Y = −56 … −42, Z = 0 … 40 (they touch the ring's outer surface and the blade's side).
5. **Bolt clearance holes ×4** in the lugs, axis along X: Ø**4.3** through, at Y = ±49, Z = **12** and **30**. Counterbore for an M4 socket head is *not* wanted (heads sit proud on the +X face).
6. **Bore cut.** Remove the pole's bore profile (Ø70.2 round half; the cap carries the crest) through the whole part, Z = 0 … 40.
7. **Split-face setback.** Remove 0.5 mm from the part at the split plane: the part's X = 0 face becomes X = +0.5 everywhere it faces the cap (ring wall and lugs). Together with the cap this gives a 1 mm clamp gap when the bore is at nominal size.
8. **LED channels ×4** cut into the underside (Z = 0 face), depth **2.6** (Z = 0 … 2.6): X = **56 … 208**, width **8.4**, centrelines at Y = **−21, −7, +7, +21**. Chamfer the channel mouth edges 0.8 × 45°.
9. **Junction trough** cut into the underside: X = **46 … 56**, Y = **−28 … +28**, depth **8** (Z = 0 … 8), inside corners R1. Around its mouth add a **1.5-deep rebate** for the cover: X = 45.8 … 56.2, Y = −28.2 … +28.2, Z = 0 … 1.5.
10. **Zip-tie post** inside the trough: Ø3 pin at (51, −20) from the trough ceiling (Z = 8) down to Z = 2.
11. **Cable passage**: a Ø**6** hole along −X from the trough's back wall into the bore: axis at Y = 0, Z = **4**, running from X = 46 to X = 35.1 (it breaks through the ring wall). Chamfer both ends 1 × 45°.
12. **Decorative grooves** on the ring's outer surface, exposed part only: three grooves 1 wide × 1 deep at Z = **20, 26, 32**, full circumference of the half ring.
13. Chamfer the ring's top outer edge 1 × 45°; break all lug edges 0.5.

### Part 2 — Cap (one print, bronze or charcoal PLA)

1. **Sleeve.** The pole's Dune profile offset outward by **7** (so a 7 mm wall), extruded Z = 0 … 40, kept for X ≤ 0 only. On the circle side this is the R42 ring; around the crest it is two R77 arcs (centres (0, ∓35)) meeting at a nose rounded about R13 — the nose wall ends up 8 thick at Y = 0.
2. **Lugs ×2.** Blocks X = **−10 … 0**, Y = +42 … +56 and Y = −56 … −42, Z = 0 … 40.
3. **Heat-set insert pockets ×4** from the split face along −X: Ø**5.6**, depth **8**, at Y = ±49, Z = 12 and 30 (for M4 inserts).
4. **Bore cut.** Remove the pole's bore profile (crest half, +0.1 offset).
5. **Split-face setback** 0.5 mm, as on part 1 (the cap's X = 0 face becomes X = −0.5).
6. Grooves at Z = 20, 26, 32 continuing part 1's; nose edges rounded; 1 × 45° on the top outer edge.

### Part 3 — Trough cover (sand PLA)

Plate **10.0 × 56.0 × 1.5** that drops into the rebate of part 1, flush with the underside. Add four crush ribs 0.3 high × 0.6 wide × 6 long on the long sides for a friction snap (0.15 interference each side). A 2 mm fingernail notch in one short edge.

### Part 4 — Grommet (TPU, optional)

Tube Ø6.0 outside, Ø4.0 bore, 11 long, with a 1 mm lip at one end, for the cable passage.

### Hardware (not modelled, list in the BOM)

- 4 × M4 × 16 socket-head cap screws
- 4 × M4 heat-set inserts Ø5.6 × 8
- 4 × LED strips 8 × 150 mm, 12 V, adhesive-backed (2.2 thick)
- 1 × twin cable 2 × 20 AWG, Ø4
- 1 × 2.5 mm zip tie

## Assembly relationships to encode

- Part 1 and part 2 meet on the split plane with a nominal 1 mm gap, bolts along +X through part 1's lugs into part 2's inserts; heads bear on part 1's +X lug faces.
- Part 3 sits in part 1's rebate, flush with Z = 0.
- The cable: trough → passage (Z = 4) → bore → pole slot → inside the pole.
- The pole's round face is toward +X; the crest points to −X, away from the blade.

## Print orientation (drives the geometry checks)

- **Part 1**: underside (Z = 0 face) on the bed. The ring and facets rise; channels, trough and rebate are first-layer recesses. There must be **no overhang steeper than 45°** anywhere; the bolt holes are horizontal, so model them as teardrops (point up) or note "drill to Ø4.3".
- **Part 2**: split face on the bed.
- **Part 3**: flat.
- **Pole** (for whoever models it fully later): standing, 3 mm wall, in 250 mm segments with a 30 mm spigot.

## Checks to run before delivering

1. Strip fit: a 8.0 × 150 × 2.2 block fits each channel with ≥ 0.2 clearance per side and sits 0.4 proud of the channel floor, flush with the face.
2. Trough: all four channels' ends open into the trough's span (Y ±25.2 inside ±28). The cover closes flush.
3. Passage: Ø6 at Z = 1 … 7 clears both the trough ceiling (Z = 8) and the underside; it breaks into the bore only.
4. Clamp: with the pole at nominal size the split gap is 1.0; with the bore closed by 0.5 the halves still do not touch anywhere else.
5. Wall thicknesses ≥ 3 everywhere except the stated 8 mm edge wall and 7 mm ring; report any spot thinner than 2.
6. Mass of part 1 with 30 % infill under 220 g; centre of mass of the full head within 90 mm of the pole axis (report the numbers).
7. No overhang > 45° in the stated print orientations; report any.

## Deliverables

- `deck_half.step/.stl`, `cap.step/.stl`, `trough_cover.step/.stl`, `grommet.step/.stl`, `pole_stub_reference.step` (200 mm, slot included), `arrakis_rail_head_assembly.step`.
- A short report: the check results above, the plateau polygon you obtained from the chamfer, any dimension you had to change and why.
- Parameters to expose: `BORE_PROFILE` (dune | round | reuleaux), `BLADE_OUTLINE` (hawk | crest | shield), `HAND` (left | right), `STRIP_WIDTH` (8), `STRIP_LENGTH` (150), `POLE_D` (70).
