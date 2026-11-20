# PCBWay / KiCad fabrication plan — Rev C

## Target (not final)
- 90 x 60 mm, 4-layer, 1.6 mm FR4, 1 oz copper, SMD assembly where practical; four ~3.2mm mounting holes with clearances to isolation zones. These dimensions are *planning targets* not CAD-frozen.
- Group 1: J1 and EMI/surge/reverse/supply regulator on far side of ESP antenna. Group 2: ESP32-S3, USB-C, RESET/BOOT, test pads. Group 3: J2 with optocoupler LED/cathode side near field connector. Group 4: dedicated output domain, U4, U7/U8, U9/U10, J3/J4.
- RF antenna should project off a free board edge in air, not under copper, metal, or enclosure ribs. Observe vendor keep-out across all layers and around antenna module.
- L2 continuous logic reference where legal; split all layers over output isolation barrier. Copper-to-copper spacing initially >=4mm goal, NOT a verified safety clearance; use connector/power converter/optocoupler package creepage limits and applicable standards to finalize.
- Keep switcher VIN/SW/inductor high-current loops tight, preserve TI recommended top-layer thermal and ground routes, separate feedback/sense from switching field and USB.
- Avoid routing USB data through open isolated gap. Keep return pairs from servo buffers to their connectors close; surge clamp near connector with return to correct ground side.
- Do NOT route any 24V motor bus onto the PCB or fit motor power terminals.

## PCBWay deliverable checklist: NEEDS ACTUAL KiCad CAPTURE/LAYOUT FIRST
[ ] Native `.kicad_pro`, `.kicad_sch`, `.kicad_pcb`; all schematic symbols have correct PAD numbers, pin types and footprints.
[ ] Electrical rules check with acknowledged, justified exceptions only; ERC report.
[ ] Exact approved MPN per BOM line; stocked parts / assembly supply, footprint and solder method determined.
[ ] PCB traces/planes placed and routed; no copper spanning intended isolation; DRC report.
[ ] Gerbers for F.Cu, In1.Cu, In2.Cu, B.Cu, solder masks/pastes, silkscreens, edge cuts; Excellon drill for plated and non-plated.
[ ] IPC/PCBWay centroid pick-and-place and BOM designator alignment; assembly drawing and polarity photos.
[ ] Appropriate stencil, fiducials, assembly panel and test points; board orientation markings and serial number space.
[ ] Confirm manufacturer capabilities for converter, ESP module and exposed-pad buck reflow; review inductors/large connectors requiring hand assembly.
[ ] Bring-up fixture with current-limited +12V, simulated +12V switches and PWM scope test points.
[ ] Prototype pilot build 2-5 units; complete validation of power transients/USB/WiFi/servo before production.

**Status:** This package contains no KiCad source, Gerbers or CPL. Do not issue a production order yet.
