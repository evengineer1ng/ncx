# RB15 livery — artist reference kit

Generated 2026-09-18 from the installed RB15 F1 Car Mod Demo v10 (Nexus 31543, `fable5.archive`), body mesh
`rb15_body.mesh`, material `rb15_ext`. Everything here describes the **one texture** the livery system swaps:
`2019_f1_red_bull_rb15_ext_23.xbm` (4096 × 4096, BC7, sRGB).

## Contents

| File | What it is |
|---|---|
| `templates/rb15_ext_uv_template_4096.png` | Transparent 4096² UV wireframe (1 px black lines) of every livery-bearing triangle. Put it on top of your layer stack. |
| `templates/rb15_ext_uv_regions_4096.png` (+ `_2048.jpg`) | Atlas colour-coded by car surface with a legend (neutral background). Region split is derived from 3D position; big islands are exact, the sidepod/engine-cover border is approximate. |
| `templates/rb15_ext_uv_mirror_4096.png` (+ `_2048.jpg`) | Islands whose UV winding is flipped (red). Text/logos painted there read backwards in game unless you pre-mirror them. Computed, not eyeballed — confirm with the probe below. |
| `test-liveries/rb15_ext_probe.png` | Orientation probe: 8×8 grid of labelled cells (A1…H8), each with an asymmetric **F** glyph and a ▼ arrow pointing to +V (down the atlas). Build it as a livery, drive around, and each panel tells you which atlas cell it samples, whether it's flipped (backwards F) and how it's rotated (arrow). Definitive answer for text placement. |
| `test-liveries/rb15_ext_ncm_test.png` | The exact source PNG of the green/magenta "NCM TEST" livery that passed milestone 1. Same pipeline as your real art. |
| *(not in this public copy)* | The local kit additionally holds the mod author's stock textures and a wireframe-over-stock-art map; those are not redistributed. Uncook them yourself from your installed `fable5.archive` with WolvenKit (`uncook -r "\.xbm$" --uext png`) if you want them as a reference layer. |

## Export spec (what to hand to the build)

4096 × 4096, 8-bit RGB PNG, sRGB, **opaque** (alpha is ignored by the material; flatten), no template layers visible,
named `rb15_ext_<name>.png` with `<name>` = lowercase `[a-z0-9_]`. Drop into `../liveries/` and run
`../tools/build_liveries.py --deploy`. Texel density is ~6–7 px per cm of car surface on every major panel
(front/rear wing 7.1, nose 7.0, engine cover 6.6, sidepods 6.3, floor 5.7), so 4096 is the right working size;
fine text below ~3 px stroke will not survive BC7 + mips.

## Which Formula-car surface is where (atlas pixel coordinates, origin top-left, V increases downward)

| Surface | Atlas location (approx bbox, px) | Mesh chunk | Notes |
|---|---|---|---|
| **Front wing** (main plane, flaps, both endplates) | top-left, (27,40)–(1948,996) | 31 | "Red Bull Racing" endplate art top-left; striped flap planes to its right; "Aston Martin" strips (top centre) = endplate outer faces. Both endplates are separate islands. |
| **Rear wing** (main plane, upper flap, endplates, DRS) | top-right, (1960,27)–(4072,3302) | 18 | Big "RedBull" = main plane rear face; yellow shapes = endplate tips; "23" numbers top-right = endplate number boards. |
| **Nose + front bulkhead** | left-centre column, (31,1490)–(1368,3628) | 13 | Yellow/red RB bull = nose top; long tapered strip = nose underside/sides. |
| **Front suspension fairings / brake ducts** | scattered small islands, mostly left-centre and upper-right | 13, 15, 17 | Small, thin — solid colour or simple stripes only. |
| **Sidepod LEFT** (inlet, side, top) | right-centre, (403,92)–(4046,2380) | 13 | Big "RedBull" side lettering + Honda block. |
| **Sidepod RIGHT** | right-centre / lower-right, (403,92)–(4084,3946) | 13 | Its own islands (see "shared" below) — asymmetric liveries are possible. |
| **Engine cover / shark fin / airbox** | centre, (514,1286)–(3742,3832) | 13 | Large red-tinted area on the regions map; airbox top has the yellow rectangle (stock art's ERS/HUD panel spot). |
| **Cockpit surround / halo** | centre-left, (36,1670)–(3422,3448) | 13 | Halo is a thin tube: keep to solid colour. |
| **Floor / undertray / bargeboard tops** | lower-left and right edges, (1760,104)–(4084,4058) | 13 | Rarely visible; large "Red Bull" lower-left block is the rear-wing-facing floor edge + rear crash structure. |
| **Rear crash structure / diffuser** | right edge, (3342,1146)–(4066,3972) | 13 | Small. |
| **Mirrors** | (408,1754)–(1580,3346) | 20, 26 | Two separate islands (L/R). |
| Tiny trim (wing pylons, fasteners, camera pods) | scattered | 19, 27, 28, 30 | Solid colour only. |

Region sizes in triangles: nose 2164, rear wing 2142, sidepod L 2123 / R 2081, engine cover 2093, cockpit/halo 1735,
floor 1320, front wing 1148, susp/ducts 1027, mirrors 880, diffuser 684, chassis sides 124.

## Overlapping / shared islands

Measured overlap between region masks is ≤1 % of the smaller region in every pair (nose↔front-susp 2124 px,
susp↔chassis-sides 2149 px, engine-cover↔diffuser 2258 px — all seam slivers from the heuristic region split).
**Left and right sidepods share only 277 px** (0.03 %). Practically: **every major panel has its own atlas space
and can be painted independently, including left vs right.** No part of the livery texture is reused by two
different panels.

## Mirrored islands

2 710 of 18 015 livery triangles (15 %) have flipped UV winding — red in `rb15_ext_uv_mirror_*.png`. They are
mostly the *second* side of symmetric pairs: one rear-wing endplate + flap tip, one front-wing endplate ("Esso"
block, lower-right), the right half of the rear-wing main plane ("Bull"), part of the nose underside, one sidepod
number board ("23" right edge), one mirror. Text you place there will read **backwards** in game unless you mirror
it in the source. Upside-down text on the *stock* atlas (e.g. "ASTON MARTIN", "HONDA") is a *rotated* island, not
a mirrored one — paint it upside-down on the atlas exactly as the stock art does and it reads correctly on the car.
Because winding analysis can be fooled by inward-facing surfaces, treat the red map as a strong hint and use the
probe livery for a definitive check before finalising sponsor text.

## Panels that cannot be painted from this texture

These are separate geometry with their own textures; they keep the stock look no matter what livery you load:

- **Sponsor badge plates** — mesh chunks 22 (whole car, 409 tris) and 24 (front wing, 1832 tris), material
  `rb15_badges`, texture `2019_f1_red_bull_rb15_badges.png` (1024²). Floating decal meshes carrying Red Bull era
  sponsor logos. If they clash with your livery, the *same* add-on mechanism can swap this texture too (a second
  material in the patch mesh) — not built yet; ask when you get there.
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
