# KiCad modernization and transfer validation

Validated on 2026-10-20 using KiCad 10.0.7 and FreeCAD 1.1.4.

## Opening the project

Open `radio-86rk-rom.kicad_pro`. The active schematic is `radio-86rk-rom.kicad_sch`; the PCB is `radio-86rk-rom.kicad_pcb`. The legacy `.pro`, `.sch`, `.net` and cache files remain for reference. Historical PDFs and fabrication outputs have not been regenerated.

The original cached symbols were converted with KiCad's native tools. All 16 placed symbols resolve to the nine definitions in `radio-86rk-rom-symbols.kicad_sym`. All nine placed footprints resolve to eight files in `radio-86rk-rom-footprints.pretty`. The six component model paths resolve to `radio-86rk-rom.3dshapes`. Both library tables use `${KIPRJMOD}`; no external custom library is needed for these references.

The symbol geometry comes from the original cache and the footprints from the original PCB. Footprint assignments were backannotated to the schematic. Component values were preserved, and the missing power-symbol reference was annotated with KiCad's native tools.

## Validation results

- The modern schematic has exactly the same 28 groups of connected component pins as the original published `.net` file.
- Footprint positions, 63 pads and their net assignments, 183 track segments, three vias, board outline and zone outlines/net assignments match the first native-converted PCB baseline.
- All local symbol, footprint and six model references resolve.
- A minimal copy containing only the active project and its local dependencies successfully exported a populated STEP assembly from a different folder.
- The native KiCad 3D render shows U1, its DIP-28 socket, J1, JP1, JP2 and C1 aligned to the board.
- FreeCAD imported the STEP as seven valid solids: the board and six component bodies, including the U1 socket. The saved `.FCStd` was closed, reopened and checked successfully.

Current outputs are in `exports/`: `radio-86rk-rom.step`, `radio-86rk-rom.FCStd` and `radio-86rk-rom-3d.png`. They represent the current migrated project. Older exports outside that folder were retained.

## Remaining rule findings

Missing component references are resolved. The design does not yet have a clean electrical or PCB rule check. Rules were not relaxed and copper was not rerouted during this migration.

| Check | Errors | Warnings | Findings |
| --- | ---: | ---: | --- |
| PCB DRC | 6 | 90 | Six GND copper-to-edge clearances; silkscreen/text warnings |
| Schematic ERC | 1 | 24 | JP1 input pin without an output driver; legacy bus-membership warnings |

There are zero unconnected PCB items.

### PCB errors

All six are front-copper GND tracks with 0.4600 mm clearance against a 0.5000 mm board-edge rule: a 0.0400 mm shortfall. These remain at the original routed locations.

| Board edge | Track start X, Y (mm) |
| --- | --- |
| Left, X = 114.3 mm | 114.935, 106.68 |
| Left, X = 114.3 mm | 117.475, 109.22 |
| Left, X = 114.3 mm | 114.935, 97.4725 |
| Top, Y = 88.9 mm | 137.795, 89.535 |
| Top, Y = 88.9 mm | 139.7, 91.44 |
| Top, Y = 88.9 mm | 123.770457, 89.535 |

The 90 warnings comprise three silkscreen-to-edge clearances, ten text-height findings, 63 silkscreen-over-copper findings and 14 silkscreen overlaps.

### Schematic error

ERC reports `pin_not_driven` at JP1 pin 2 (name `C`, type `Input`), located at 133.35, 115.57 mm. The original cached jumper symbol declares this center pin as an input. Review that pin type together with the documented jumper configurations before correcting or excluding this finding. No artificial driver or waiver was added to silence it.

The remaining 24 schematic warnings are `net_not_bus_member` findings associated with the legacy bus notation.

## Model limitations

The six component models are generic KiCad packages, not manufacturer-verified assemblies. In particular, jumper caps and the mating cable are absent. The U1 socket is included, with the chip raised 3.627 mm above its original model position to seat it on the socket. See `radio-86rk-rom.3dshapes/README.md` for package details, the small capacitor pitch difference and licensing. The seven-solid transfer verifies file integrity, not manufacturing readiness or complete mechanical fit.

## Recovery and evidence

A complete pre-edit backup was retained outside the repository before migration. The modernization is recorded on branch `cad/kicad-10-modernization`.

The connectivity and geometry comparison is included in `validation/verification.json`; FreeCAD solid and reopen results are in `validation/freecad-transfer.json`. Detailed DRC/ERC reports and intermediate recovery evidence are retained in the local audit workspace outside this repository.

During migration, the MCP symbol-replacement operation failed after deleting JP1. The schematic was recovered through KiCad's native editor from the original legacy source, then linked to the converted original cache. The final 16-symbol count and exact 28-net connectivity comparison verify that recovery. The failed intermediate file is retained only in the external evidence folder and is not an active project source.
