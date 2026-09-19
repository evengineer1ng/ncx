# RB15 livery — artist reference kit

> **ERRATUM 2026-09-18 (evening) — all templates regenerated.** The first kit's templates were **vertically
> flipped** relative to what the game samples (the atlas is nearly top/bottom symmetric, which hid it until the
> in-game probe). Everything in this folder is now in the correct orientation and was validated by rendering the
> probe and the stock Red Bull art onto the mesh and matching them against in-game screenshots
> (`reference-renders/`). If you painted on the old template: flip your whole design vertically once, then re-do
> any text using the side rules below.

Generated 2026-09-18 from the installed RB15 F1 Car Mod Demo v10 (Nexus 31543, `fable5.archive`), body mesh
`rb15_body.mesh`, material `rb15_ext`. Everything here describes the **one texture** the livery system swaps:
`2019_f1_red_bull_rb15_ext_23.xbm` (4096 × 4096, BC7, sRGB).

## Contents

| File | What it is |
|---|---|
| `templates/rb15_ext_uv_template_4096.png` | Transparent 4096² UV wireframe (1 px black lines) of every livery-bearing triangle. Put it on top of your layer stack. |
| `templates/rb15_ext_uv_regions_4096.png` (+ `_2048.jpg`) | Atlas colour-coded by car surface with a legend (neutral background). Region split is derived from 3D position; big islands are exact, the sidepod/engine-cover border is approximate. |
| `templates/rb15_ext_uv_mirror_4096.png` (+ `_2048.jpg`) | Islands whose UV winding is flipped (red). Text/logos painted there read backwards in game unless you pre-mirror them. Computed, not eyeballed — confirm with the probe below. |
| `reference-renders/probe_reference_sides.jpg`, `probe_reference_top.jpg` | The probe livery rendered onto the mesh from left / right / rear / front / top, in the same orientation the game shows it. Find a panel, read its cell label and the F glyph: that is exactly how anything you paint in that cell will appear. |
| `test-liveries/rb15_ext_probe.png` | Orientation probe: 8×8 grid of labelled cells (A1…H8), each with an asymmetric **F** glyph and a ▼ arrow pointing to +V (down the atlas). Build it as a livery, drive around, and each panel tells you which atlas cell it samples, whether it's flipped (backwards F) and how it's rotated (arrow). Definitive answer for text placement. |
| `templates/rb15_badges_uv_template_1024.png` | Transparent 1024² UV wireframe of the sponsor/decal **plate** geometry (material `rb15_badges`). |
| `templates/rb15_badges_uv_regions_1024.png` | Plate UVs colour-coded by where the plate sits on the car (neutral background). |
| `test-liveries/rb15_badges_probe.png` | 4×4 orientation probe (A1…D4, F glyph, ▼ arrow) for the plate sheet. Pair it with any livery as `rb15_badges_<name>.png` to see which plate samples which cell. |
| `test-liveries/rb15_badges_demo_orange.png` | Example plate sheet (solid orange + "NCX") used by the `demo_orange` livery — proves plates are replaced. |
| `test-liveries/rb15_ext_ncm_test.png` | The exact source PNG of the green/magenta "NCM TEST" livery that passed milestone 1. Same pipeline as your real art. |
| *(not in this public copy)* | The local kit additionally holds the mod author's stock textures and stock-art renders/wireframes; those are not redistributed. Uncook them yourself from your installed `fable5.archive` with WolvenKit if you want them as reference layers. |

## Export spec (what to hand to the build)

4096 × 4096, 8-bit RGB PNG, sRGB, **opaque** (alpha is ignored by the material; flatten), no template layers visible,
named `rb15_ext_<name>.png` with `<name>` = lowercase `[a-z0-9_]`. Drop into `../liveries/` and run
`../tools/build_liveries.py --deploy`. Texel density is ~6–7 px per cm of car surface on every major panel
(front/rear wing 7.1, nose 7.0, engine cover 6.6, sidepods 6.3, floor 5.7), so 4096 is the right working size;
fine text below ~3 px stroke will not survive BC7 + mips.

## Which Formula-car surface is where (atlas pixel coordinates, origin top-left)

| Surface | Atlas location (approx bbox, px) | Notes |
|---|---|---|
| **Front wing** (main plane, flaps, both endplates) | bottom-left, (27,3100)–(1948,4055) | Main plane = big "Red Bull" block bottom-left; "Mobil 1" strips = endplate faces; striped flaps to the right. |
| **Rear wing** (main plane, flap, endplates) | bottom-right, (1960,794)–(4072,4068) | "ASTON MARTIN" panels = main plane faces (rear face reads from behind); "ESSO / AT&T" = endplate outer faces; "23" = number boards on the endplates. |
| **Nose + front bulkhead** | left-centre, (31,468)–(1368,2606) | "23 / TAG Heuer / Red Bull" strip = nose top; the bull silhouette strips = nose sides/underside. |
| **Sidepod LEFT** (driver's left) | centre-right, lower band (403,1716)–(4046,4003) | Upright "RedBull" + "HONDA" in the stock art — text painted upright reads upright. |
| **Sidepod RIGHT** | centre-right, upper band (403,150)–(4084,~2000) | Upside-down "RedBull" + "HONDA" in the stock art — paint text rotated 180°. |
| **Engine cover / airbox / shark fin** | centre, (514,264)–(3742,2810) | Big bull logo = engine cover top/sides. |
| **Cockpit surround / halo / chassis flanks** | centre-left, (36,648)–(3422,2426) | Halo = thin tube, solid colour only. "ASTON MARTIN" chassis strips: upper = right side (180°), lower = left side (upright). |
| **Floor / undertray** | scattered along top and right edges | Rarely visible. |
| **Rear crash structure / diffuser** | right edge, (3342,124)–(4066,2950) | Small. |
| **Mirrors** | (408,750)–(1580,2342) | Two islands (L/R). |
| Tiny trim | scattered | Solid colour only. |

Region sizes in triangles: nose 2164, rear wing 2142, sidepod L 2123 / R 2081, engine cover 2093, cockpit/halo 1735,
floor 1320, front wing 1148, susp/ducts 1027, mirrors 880, diffuser 684, chassis sides 124. Texel density 5.7–7.1 px/cm.

## Orientation rules (validated in game)

- **Left-side panels** (driver's left, the side with the upright stock text): paint text/logos **upright** — they read as painted.
- **Right-side panels**: paint text/logos **rotated 180°** (exactly like the stock "HONDA" / "RedBull" upper islands). Not mirrored — rotated.
- **Rear-wing endplate outer faces**: upright as painted (probe cells G5/G8 read upright).
- **Engine cover / airbox top**: read from the car's left, upright as painted (probe D7/D8).
- **Rear-wing main plane rear face** and **front-wing top**: check the cell in `probe_reference_sides.jpg` (REAR / FRONT rows) before placing text — several flap surfaces are rotated 90°.
- Genuinely **mirrored** islands are only 193 of ~18 000 triangles (~1 %, tiny trim) — the red map is almost empty; the earlier "15 %" figure was wrong.

## Overlapping / shared islands

Measured overlap between region masks is ≤1 % of the smaller region in every pair (nose↔front-susp 2124 px,
susp↔chassis-sides 2149 px, engine-cover↔diffuser 2258 px — all seam slivers from the heuristic region split).
**Left and right sidepods share only 277 px** (0.03 %). Practically: **every major panel has its own atlas space
and can be painted independently, including left vs right.** No part of the livery texture is reused by two
different panels.

## Mirrored islands

Only 193 of 18 015 livery triangles (~1 %) have flipped UV winding — small trim pieces, shown red in
`rb15_ext_uv_mirror_*.png`. No major panel is mirrored. What *looks* mirrored in earlier tests was the right-side
panels being **rotated 180°** — see the orientation rules above. Use the probe renders for the definitive answer on
any panel.

## Panels that cannot be painted from this texture

These are separate geometry with their own textures; they keep the stock look no matter what livery you load:

- ~~Sponsor badge plates~~ — **now paintable per livery**, see "Badge / decal plates" below.
- **Suspension arms, bargeboards, halo mounts, wheel fairings, floor edge details** — chunks 1/3/5/8/14/21/25,
  material `rb15_misc` (2048²).
- **Cockpit interior + front-wing underside plate** — chunks 12/23, material `rb15_cab`.
- **Lights, chassis tub** — `rb15_lights`, `rb15_chassis` (small).
- Wheels/tyres/rims are separate meshes with their own textures (`..._wheel.xbm`, `car_tyre_slick_pirelli_f1_2019.xbm`).

Everything else you see on the car body — nose, sidepods, engine cover, wings, floor, mirrors, halo — is the livery
texture.

## Material facts that affect how art reads in game

`metal_base.remt`, `BaseColorScale 1,1,1,1`, `RoughnessScale 0.5`, `MetalnessScale 0`: uniform semi-gloss paint,
no normal/roughness/metal maps. Consequences: matte/gloss variation can't be painted (all panels share one
roughness); metallic flake is not possible; carbon-fibre weave has to be painted as colour only; dark colours look
good, pure white (255) will bloom in sun — keep highlights ≤ 235.

## Badge / decal plates (`rb15_badges`, optional per livery)

The RB15 carries floating plate geometry on top of the body, textured from a separate **1024 × 1024 decal sheet**
(`2019_f1_red_bull_rb15_badges.xbm`, BC7, sRGB). It is *not* a sponsor grid: the stock sheet holds a large
**Red Bull Racing / Formula One Team** logo plate, a **HIGH VOLTAGE** warning triangle, an **"E"** electrical
roundel, and a couple of hex fastener caps. Plate geometry: chunk 24 (front-wing plates, 1832 tris — the small
wing-mounted plates), chunk 22 (409 tris of plates over nose, sidepods L/R, engine cover, mid-chassis and tail),
chunk 16 (rear-wing plates, 56 tris). Same flat `metal_base` material as the body (no alpha — plates are opaque).

**How to supply one:** save a 1024 × 1024 RGB PNG next to your livery as `liveries/rb15_badges_<name>.png`.
The build then clones the plate material for that livery only. If the file is absent the build uses
`liveries/rb15_badges_default.png` when that exists, otherwise the plates keep the author's stock sheet
(nothing is patched — proven safe by the `ncm_test` livery). `ncm_rb15_liveries.manifest.json` records which of
`own | default | stock` each livery got.

**Painting guidance**
- Keep the stock sheet as a reference layer and paint replacements *in the same spots*: your team logo where the
  Red Bull Racing plate is, keep or restyle the HIGH VOLTAGE / E safety badges, recolour the fastener caps.
- Several plates' UVs **overlap** on this sheet (the regions map shows a large nose-plate polygon spanning the
  logo/E area and the front-wing slivers over the E roundel), so unlike the body atlas the plate sheet is *not*
  cleanly partitioned. Treat it as a decal sheet and use `rb15_badges_probe.png` in game once to see exactly which
  plate shows which cell before committing text.
- Plates cannot be hidden by texture alone (opaque material). To "remove" a plate, paint it in the body colour of
  that area so it disappears visually.
- Export spec is the same as the body: 8-bit RGB, sRGB, opaque; stay at 1024 (the geometry is small, more
  resolution buys nothing).
