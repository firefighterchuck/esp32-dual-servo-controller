# ESP32-S3 vehicle dual-servo controller — Revision C
**Design frozen for schematic capture:** 2026-11-20  
**Primary installation:** One DSSERVO RDS51160 at 24 V, fed from the owner's existing external nominal 12 V -> regulated 24 V / 240 W converter.  
**Expansion:** All two optically isolated switch inputs and both 5 V, optically isolated servo command outputs installed on controller PCB.  
**Status:** Electrical design specification / capture handoff. NOT a finished KiCad schematic, routed PCB, validated design, or orderable PCBWay PCBA package.

## What's new from Rev B
- Servo model corrected to RDS51160: 19.2-25.2 V motor supply; 24 V nominal; 4.2 A published stall at 24 V.
- Existing 240 W converter documented as an external unit, **never routed through the controller PCB**.
- Only PWM output 1 is commissioned at installation. Output 2 is fully fitted but firmware-disabled by default.
- USB, control GPIOs and their exact ESP32-S3-WROOM-1 module PAD numbers documented from manufacturer's pin table.
- Isolated 5 V power module pad numbers corrected and documented; its recommended 4.7 uF input / 10 uF output capacitance included.
- Optional vehicle input voltage monitor allocated to GPIO6 / ADC1_CH5 (must validate divider/clamp).
- Release gates for switching power reference design, harness connectors, automotive transients, isolation and servo behavior are explicit.
- A four-page vector electrical reference drawing, net-to-pin capture map, preliminary BOM, wiring guide and acceptance tests included.

## Files
- `01_System_Requirements.md` — immutable use case and behavior
- `02_Interface_and_Pin_Map.csv` — controller pins and power-domain allocations
- `03_Connector_Pinout.csv` — planned external connections and explicit power warning
- `04_Electrical_Connections.csv` — proposed net/pin capture instructions
- `05_BOM_RevC_Preliminary.csv` — placed parts candidates, notes and engineering holds
- `06_Schematic_Capture_Notes.md` — detailed circuit-level capture requirements
- `07_PCB_Floorplan_and_Mfg_Notes.md` — layout constraints, fabrication and assembly requests
- `08_Validation_And_Bringup.md` — electrical, safety, EMC and mechanism test sequence
- `09_Firmware_Behavior.json` — initial firmware configuration schema, no fixed servo endpoints
- `10_Manufacturer_References.md` — primary source URLs
- `RevC_Electrical_Reference_Drawings.pdf` — four pages of schematic-reference diagrams and wiring architecture
- `RevC_Wiring_Overview.png` — external installation drawing
- `12_USB_Self_Powered_Review_Hold.md` — USB self-powered VBUS detection concern
- `13_Build_Request_Notes.md` — RFQ / electronics designer handoff details

## Before ordering PCBA
This package deliberately does **not** include .kicad_sch, .kicad_pcb, Gerbers, drill files, stencil layers or pick-and-place, because those must come from a completed and checked CAD layout. Do not upload this ZIP as a production-ready assembly order. Do not use it to connect servos before electrical verification.
