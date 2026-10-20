# Component models

These six STEP files are unmodified standard models copied from the KiCad 10.0.7 installation on 2026-10-20. `sources.json` records their source filenames and SHA-256 hashes. The source installation paths are provenance only; the project resolves the bundled files through `${KIPRJMOD}`.

| Reference | Package model |
| --- | --- |
| U1 chip | DIP-28, 15.24 mm row spacing |
| U1 socket | Through-hole DIP-28 socket, 15.24 mm row spacing |
| J1 | Shrouded vertical IDC header, 2 × 13, 2.54 mm pitch |
| JP1 | Vertical pin header, 1 × 3, 2.54 mm pitch |
| JP2 | Vertical pin header, 1 × 2, 2.54 mm pitch |
| C1 | Rectangular MLCC, 7.2 × 2.5 mm body, 5.00 mm lead pitch |

These are generic package representations, not verified CAD for the BOM manufacturer part numbers. C1's footprint pitch is 5.08 mm; the centered 5.00 mm model differs by 0.04 mm per lead. Models have not been scaled to hide that difference. The socket is included beneath U1. Both models share the same XY offset and rotation; the chip is raised by 3.627 mm, aligning its body underside (model Z = 0.38 mm) with the socket contact tops (model Z = 4.007 mm). This is a generic seated representation, not a manufacturer-specific fit check. Jumper caps and the mating ribbon-cable assembly are not modeled. Allowances based on those parts require their actual dimensions.

The models retain the KiCad library license in `LICENSE.md`, including the KiCad exception. Official source: https://gitlab.com/kicad/libraries/kicad-packages3D . Model files remain unchanged; their placements are stored in the PCB and local footprint library.
