# Manufacturing handoff / engineering RFQ note

Project: ESP32-S3 dual optocoupled vehicle switch input / isolated 5V servo-control board, Rev C. Intended prototype lot: 3 to 5 PCBA units, qty dependent on sourcing and test budget.

1. This is an **electrical reference design/specification**, not yet an orderable PCBA.
2. Proposed board: four-layer FR4 ~90x60mm x1.6mm, 1oz copper, SMT components, 4 mounting holes, keyed signal-only connectors.
3. MCU: ESP32-S3-WROOM-1-N8. Two 12V opto-isolated input circuits. Two 5V opto/logic-buffer servo signal outputs. External 12->24V 240W boost converter and servo motor are NOT included in board assembly.
4. Require vendor/design contractor to capture a native KiCad schematic, resolve every BOM HOLD, complete an ERC/DRC-clean 4-layer layout, review automotive input surge, RF and isolation copper clearances, generate fab/assembly files, and provide prototype bring-up report.
5. Request separate quote for engineering design/review, fabrication/assembly, and test/firmware work. PCB fab vendors may not provide engineering design by default.
6. After design signoff: deliver KiCad source, Gerbers, drill, centroid/CPL, exact MPN BOM, assembly drawings, tested prototype and firmware binary/source.
7. Primary acceptance: powered by vehicle 12V nominal with programmed position control to one RDS51160; second hardware PWM channel populated and electrically tested, firmware disabled until commissioned.
