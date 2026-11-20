# Functional requirements and design decisions — Rev C

1. One independent 12 V-vehicle supply J1 powers the controller, not the servo motor. Use upstream harness fusing, a low-current on-board fuse/protection stage, reverse-polarity and automotive transient mitigation, then a 5 V regulator and separate 3.3 V regulator.
2. Inputs: two positive-switched +12 V vehicle signals with J2 shared switch return. Each channel is optocoupled, hardware active-low at ESP32; deglitching is handled by RC filter and firmware >=20 ms debounce.
3. Outputs: two independent 5 V positive-pulse command outputs through fast 6N137 optocouplers and 74AHCT1G14 Schmitt inverter buffers. Separate output power from isolated +5V converter. Both signal returns share the isolated converter output return, not an on-board bridge to controller ground.
4. Servos: only RDS51160 servo #1 connected initially; #2 output present on PCB, disabled in firmware until commissioned. Motor supply from customer's external 12 V -> 24 V nominal, 240 W converter and appropriate external branch protection. Servo current does not travel on controller PCB.
5. 24 V converter is an existing unit with only rating provided; input window, transient tolerance, efficiency, inrush, measured 24 V output and peak regulation are not yet independently verified.
6. Command: switches select between position pulse widths, NOT different PWM frequencies. Each output has a 50 Hz default repeat period (20 ms); pulse width in the 500..2500 us servo-supported range. First commissioning should use tighter measured mechanical endpoints and individually programmable OFF/ON positions.
7. Boot/failure: servo output LOW by circuit design when MCU does not drive opto LEDs; valid servo pulses begin only after firmware checks configuration and debounces switch states. Servo physical behavior under missing signal must be empirically verified. Loss of pulses does not prove motor torque is removed.
8. One external motor-power disconnect / appropriate fuse is required for safe testing. This PCB is not a safety-certified motor controller. Separate kill circuit if mechanism can injure someone or damage equipment.
9. 3.3 V rail supply capacity >=500 mA as Espressif recommends, 5 V controller supply sized for 3.3 V converter input plus isolated converter and headroom.
10. USB-C data-only; 12 V vehicle supply must be applied for programming. USB VBUS MUST NOT energize the +5V_LOGIC rail; keep USB connector shield and GND engineered for ESD/EMC.
11. Optional battery-voltage diagnostic from protected supply to ADC1 GPIO6, with divider/filter/clamp verified at abnormal input surges. It is not a safety input.
12. Layout: four layers, initial 90 x 60 mm envelope, isolated servo-signal copper domain, ESP32 antenna edge keep-out. PCB size/mechanics are preliminary.

### Important isolation limit
Even if the PCB maintains separate copper domains, a non-isolated 12-to-24 V boost converter typically connects its motor return to vehicle return. Connecting J3/J4 signal return to servo return may then connect the output-side domain back to controller/chassis ground outside the PCB. Thus optocouplers are included as requested, but full **vehicle-wide galvanic isolation is not guaranteed**.

### Release criterion
Full KiCad project, exact approved MPNs/footprints, automotive input power-stage electrical review, ERC, completed PCB routing, DRC, EMI and vehicle transient review, assembled-prototype validation are mandatory before PCBWay assembly release.
