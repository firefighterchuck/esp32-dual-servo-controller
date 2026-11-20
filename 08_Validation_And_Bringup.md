# Prototype verification & release acceptance — Rev C

## Stage 0: design review
- [ ] Verify 24 V servo converter model, input operating window, output max/ripple, startup overshoot, continuous derating and current rating.
- [ ] Approve TVS selection/reverse polarity topology/fusing for vehicle electrical pulse categories, operating temperature and installation harness.
- [ ] Independently review both TI regulator external networks for 5.00 V and 3.30 V output, inductor thermal/current margins, capacitors under DC bias, switch-node loop, bootstrap/ripple network and power sequencing.
- [ ] Review all exact manufacturer pin tables, module pads and assembled footprint pin-1 orientation; complete KiCad ERC/DRC.

## Stage 1: controller bench without servo power
1. Inspect orientation/footprints/isolation moat and all power rail shorts.
2. Current-limit external supply and apply 9, 12, 14.8, 16 V inputs; observe VIN, +5V_LOGIC, +3V3_LOGIC, +5V_SERVO_ISO, GND_SERVO_ISO on scope.
3. Check isolated domain float resistance with no USB/servo attached. Verify protected 3.3V bus and ESP32 start time at supply ramp/fall.
4. Plug USB into independently powered controller. Confirm data enumeration and no USB backfeed to 5V regulator with J1 off. Check reset, boot and UART recovery.
5. Apply simulated 12V switch signals individually, including slow contact bounce and off-state open-circuit. Measure GPIO4/5 logical lows and debounced state transitions.
6. With both output optos fully powered, scope J3/J4: 0-to-5 V levels, 50 Hz frame, stable 500..2500 us pulse capability, startup LOW, no unwanted high pulses on brownout/resets/firmware update.
7. Confirm channel 2 hardware works with test command but firmware keeps output 2 disabled until commissioned.
8. Load +5V_SERVO_ISO to expected range: confirm converter 15mA min load via 270R, full rail thermal margin and regulator cross-coupling.
9. Verify optional VIN monitor at 9..16 V and under designed abnormal transient simulation; GPIO6 never sees unsafe voltage.

## Stage 2: servo power — mechanical clearance and cutoff
1. Connect the external converter to lab supply or vehicle 12V branch with appropriate fusing, without servo. Scope its 24V output on startup/shutdown, light load and step load; ensure never >25.2V steady/overshoot at servo.
2. Wire one RDS51160 servo motor only to the converter; power negative shared with J3 signal return at servo. Use no load/loose coupling to mechanical linkage first.
3. Commission conservative PWM endpoints. Verify rotation direction and mechanism hard limits at both switch positions before loading the mechanism.
4. Exercise switch 1 while switch 2 stays disconnected; confirm output 2 remains disabled. Monitor current, converter ripple, any ESP32 reset and servo cable noise.
5. Test commanded position when controller fails first, converter fails first, rapid on/off, open/broken switch wire, USB connect, Wi-Fi activity, ignition off/crank.
6. Verify physical hard-stop and independent power cutoff, since the servo can apply substantial torque and signal loss does not guarantee motion stops.
7. Only after a second servo installation is planned, evaluate simultaneous current and re-confirm combined converter capacity (not covered by the present one-servo test).

## Release decision
Do not submit PCBWay PCBA until circuit reviewed + KiCad/ERC/DRC + first prototype waveform and supply verification. Separate automotive transient/EMC testing may be needed for installation requirements.
