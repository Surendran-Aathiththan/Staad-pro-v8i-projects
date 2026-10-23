# STAAD.Pro V8i Projects

Reinforced-concrete building analysis, member design and isolated-footing design in **STAAD.Pro V8i SELECTseries 6** and **STAAD.Foundation 5.3**, with foundation drawings prepared in **AutoCAD**.

## G+4 Residential RC Building: Analysis, Concrete Design and Foundation Design

A five-storey (ground + 4) reinforced-concrete moment frame taken from a 3D analysis model through member design to isolated-footing design and foundation drawings.

![Isolated footing detail produced from the STAAD.Foundation results](G%2B4/Foundation%20Detail.png)

**Status:** ✅ Analysis, concrete design and foundation design complete (Aug 2026). See [Review notes](#review-notes) for items I would revise in a next iteration.

### Design basis

From [BASIC PARAMETERS.pdf](G%2B4/BASIC%20PARAMETERS.pdf) and the model input file [G+4.std](G%2B4/G%2B4.std):

| Item | Value |
|---|---|
| Use | Residential, RC framed structure with brick infill walls |
| Plan | 15.0 m × 12.0 m rectangle, 5 × 4 bays at 3.0 m centres (30 columns) |
| Storey heights | Ground storey 3.3 m, typical floors 3.0 m; roof at +15.3 m |
| Columns | 600 × 400 mm |
| Beams | 400 × 400 mm |
| Slabs | 150 mm, modelled as 100 plate elements |
| Concrete | M30 for design (`FC 30000` kN/m²) |
| Reinforcement | HYSD Fe415, main bars ≤ 20 mm, links ≤ 12 mm |
| Supports | Fixed at all 30 column bases |
| Allowable bearing pressure (design brief) | 200 kN/m² |

### Codes and standards

| Purpose | Standard |
|---|---|
| Concrete member and footing design | IS 456:2000 |
| Seismic load | IS 1893 (STAAD `DEFINE 1893 LOAD`) |
| Load combinations | STAAD auto-generated Indian code combinations (21 combinations) |
| Wind load | STAAD wind-load generator using its ASCE 7-10 parameter set, basic wind speed 44 m/s |

### Loads

| Load case | Definition in the model |
|---|---|
| 1, 2: EQ X / EQ Z | IS 1893 static method: zone factor Z = 0.16 (Zone III), response reduction R = 5, importance I = 1.0, medium soil (type II), 5 % damping |
| 3, 4: Wind X / Wind Z | Generated wind load, applied in both + and − directions |
| 5: Dead | Self-weight + 24.3 kN/m wall load on beams + 2 kN/m² floor load (floors +3 m to +15 m) |
| 6: Live | 3 kN/m² floor load (floors +3 m to +15 m) |

Combinations 7–27 cover 1.5(DL + LL), 1.2(DL + LL ± WL/EL), 1.5(DL ± WL/EL) and 0.9DL ± 1.5EL.

### Workflow

1. **Geometry**: 3D frame of 180 joints, 395 beam/column members and 100 slab plates.
2. **Loading**: dead, live, IS 1893 seismic and wind cases, plus auto-generated IS combinations.
3. **Analysis**: linear static analysis (900 degrees of freedom).
4. **Concrete design**: beams, columns and slab elements designed to IS 456 with concrete take-off.
5. **Foundation design**: support reactions exported to STAAD.Foundation 5.3 and 30 isolated footings designed to IS 456.
6. **Drawings**: foundation general arrangement and a typical footing detail drafted in AutoCAD.

### Key results

| Result | Value | Source |
|---|---|---|
| Fundamental period (X and Z) | 0.580 s | STAAD output, IS 1893 |
| Spectral acceleration Sa/g | 2.344 | STAAD output |
| Seismic weight | 27,848 kN | STAAD output |
| Design base shear V<sub>B</sub> (each direction) | 0.0375 × 27,848 = **1,044 kN** | STAAD output |
| Concrete volume (beams, columns, slabs) | **227.8 m³** | Concrete take-off |
| Reinforcement (beams and columns only) | 157,692 N ≈ **16.1 t** (Ø8: 37.9 kN, Ø10: 31.7 kN, Ø12: 85.4 kN, Ø16: 2.6 kN) | Concrete take-off |
| Isolated footings | 30, square, **3.05 m to 4.10 m** wide and **356 mm to 507 mm** thick | STAAD.Foundation report |
| Footing reinforcement | Typically Ø10 @ 50–75 mm bottom bars both ways | STAAD.Foundation report |
| Stability (footing 1, critical case) | Sliding ratio 10.0 to 11.5, overturning ratio 13.2 (required 1.5) | STAAD.Foundation report |

**Interpretation:** the building is relatively stiff (T ≈ 0.58 s) and the seismic base shear is about 3.75 % of the seismic weight. Interior footings are the largest because they carry the largest tributary areas; corner footings are the smallest (3.05 m).

### Deliverables

| Deliverable | File |
|---|---|
| Design basis | [BASIC PARAMETERS.pdf](G%2B4/BASIC%20PARAMETERS.pdf) |
| STAAD input model | [G+4.std](G%2B4/G%2B4.std) |
| Analysis and concrete design output (362 pages) | [Concrete design results.pdf](G%2B4/Concrete%20design%20results.pdf) |
| Isolated footing calculations (363 pages) | [Calculation Foundation.pdf](G%2B4/Calculation%20Foundation.pdf) |
| STAAD.Foundation model data | [G+4_foundation.xml](G%2B4/G%2B4_foundation.xml) |
| Foundation general arrangement | [PDF](G%2B4/Foundation%20GA.pdf) · [PNG](G%2B4/Foundation%20GA.png) · [DWG](G%2B4/Foundation%20GA.dwg) |
| Typical footing detail | [PNG](G%2B4/Foundation%20Detail.png) · [DWG](G%2B4/Foundation%20Detail.dwg) |

### Review notes

I keep these notes to show the checks I would do before issuing the design:

- **Bearing capacity:** the design brief gives 200 kN/m², but the STAAD.Foundation run used **100 kN/m²**. The footings are therefore conservative; rerunning with the site value would reduce their size.
- **Concrete grade:** member design used M30 (`FC 30000`), but the analysis material uses STAAD's default concrete (f<sub>cu</sub> ≈ 27.6 MPa, E = 21.7 GPa), and the footing design used 25 N/mm². These should be aligned.
- **Wind:** the wind loads were generated with the ASCE 7 parameter set; an IS 875 (Part 3) wind calculation would be consistent with the rest of the design.
- Column reinforcement detailing and slab reinforcement quantities are not included in the drawings or take-off.

## Opening the files

- Open `G+4/G+4.std` in **STAAD.Pro V8i** or STAAD.Pro CONNECT Edition, then run the analysis to regenerate the results.
- The PDFs and PNGs can be viewed directly on GitHub. Open the `.dwg` drawings in AutoCAD 2018 or later, or in the free Autodesk Viewer.
