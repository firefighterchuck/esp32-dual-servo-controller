# Exact schematic capture guidance (electrical design reference)

## Power domains (keep individually named)
- `VEH_BATT_POS`, `GND_LOGIC`: nominal +12 V vehicle power and return.
- `SW1_12V`, `SW2_12V`, `SW_RETURN`: two externally sourced, positive-switching 12V signals; isolated on PCB from digital return except through optocoupler light.
- `+5V_LOGIC`, `+3V3_LOGIC`: controller regulators; never supply motor or servo drive current.
- `+5V_SERVO_ISO`, `GND_SERVO_ISO`: isolated 5V output supply, referenced to servo negative at J3/J4. **DO NOT connect directly to GND_LOGIC on PCB.**
- `SERVO1_SIG`, `SERVO2_SIG`: 0-5 V servo command pulses; no 24 V electrical feed.

## 1. Vehicle power / surge protection — RELEASE HOLD
`J1+ -> harness fuse -> F1 -> reverse protection D1 -> VIN_PROTECTED -> U2 LM5164-Q1 + its official 5V reference-design passives -> +5V_LOGIC`. TVS D2 and supply filters across designated vehicle/protected nodes are selected only after ISO 7637-2 / ISO 16750-2 transients, harness and ground arrangements are known. `GND_LOGIC = J1-`.

LM5164-Q1 is automotive AEC-Q100, 6-100V and up to 1A output, but that does **NOT** by itself make the complete circuit load-dump compliant. TI EVM is configured for **12 V output**, not 5 V; do NOT copy its feedback network unchanged. The RON resistance, inductor, output LC/ripple feedback, input/filter and UVLO must be generated/checked for 5 V using TI documentation, thermal and EMI constraints. Keep its bootstrap capacitor 2.2nF/50V between BST/SW as specified.  
U3 TPS62162-Q1 takes +5V_LOGIC and generates fixed +3V3_LOGIC. Pin 5 `FB` for the fixed version is tied to AGND as TI instructs; use pin 6 `VOS` to sense 3.3 V. Verify the complete TI reference application and thermal-pad footprint. ESP32's 3.3 V supply must support >=500 mA bursts per Espressif.

## 2. Isolated 5V control output rail
`U4 IB0505XT-W75R3`: input pin 1=GND_LOGIC, pin 2=+5V_LOGIC; output pin 4=GND_SERVO_ISO, pin5=+5V_SERVO_ISO, pin8=NC (MUST remain unconnected). Datasheet reference bypass: 4.7uF/16V across input; 10uF/16V across isolated output. 270ohm/0.25W preload R209 across output 5V provides 18.5mA, meeting 15mA minimum on the current version of the converter. Confirm total load and PCB thermal derating.

## 3. ESP32-S3 pin numbers (MODULE PADS, not bare SOC)
U1 ESP32-S3-WROOM-1-N8 pads: GND 1,40,41; +3V3 2; EN 3; GPIO4 4 input1; GPIO5 5 input2; GPIO6 6 optional vehicle monitor ADC1_CH5; GPIO16 9 output1; GPIO17 10 output2; GPIO19 13 USB D-; GPIO20 14 USB D+; GPIO0 27 BOOT; UART0 RX GPIO44 module pad36 / TX GPIO43 pad37 for test pads. Observe RF antenna copper and chassis keep-out.

EN: 10k pullup to 3.3V and 1uF to GND_LOGIC, reset momentary to ground; BOOT GPIO0: 10k pullup to 3.3V, momentary to ground. Scope resets and all output pulse states on startup.

## 4. USB-C data-only
J5: data pair D- -> optional 22ohm -> U1 pad13(GPIO19); D+ -> optional 22ohm -> U1 pad14(GPIO20). Receptacle A6/B6 connect to D+, A7/B7 to D-; one 5.1k CC pull-down each CC1 and CC2. USB VBUS has no connection to +5V_LOGIC; self-powered operation requires controller vehicle supply when flashing. Review correct VBUS sensing for final USB implementation and reject connector pinouts/footprints that misconnect shell/shield. Protect D+/D- with suitable low-capacitance ESD array.

## 5. Switch input channel 1 (mirror for channel 2)
J2-1 SW1_12V -> R101 (2.7k, 0.5W) -> D101 series diode -> U5 LTV-817 pin1 LED anode. U5 pin2 LED cathode -> J2-3 SW_RETURN. Fit D103 TVS between SW1_12V and SW_RETURN with standoff/clamp selected for actual vehicle transient and jump start. U5 pin4 phototransistor collector -> GPIO4 and R103 10k to 3.3V and C101 100nF to GND_LOGIC; U5 pin3 emitter -> GND_LOGIC. Digital LOW means switch ON. Mirror with U6 / GPIO5. Confirm current-transfer ratio at lowest switch voltage and temperature; firmware debounce >=20ms.

## 6. Buffered 5V high-speed opto servo output channel 1 (mirror channel 2)
GPIO16 -> R201 220ohm -> U7 Vishay 6N137 pin2 LED anode; U7 pin3 LED cathode -> GND_LOGIC. R203=100k from GPIO16 to GND_LOGIC avoids floating opto drive. Output side: U7 pin8=ISO5V, pin5=ISO_GND, pin7=ISO5V enable HIGH, pin6=OPTO1_N pulled to ISO5V through R205=2.2k. U9 Nexperia 74AHCT1G14GW 5V Schmitt inverter: pin2 A=OPTO1_N; pin3 GND=ISO_GND; pin4 Y=buffered output; pin5 VCC=ISO5V; pin1 NC. U9 output -> R207 220ohm -> J3-1 servo signal. J3-2 = ISO_GND -> servo's own signal return / motor negative. Fit output low-capacitance transient clamp after reviewing cable ESD; no 24V ever connects here. U7 and U9 each need 100nF local decoupling.

With LED OFF: U7 open drain releases high -> U9 output LOW. With LED ON: U7 pulls LOW -> U9 output HIGH. Verify the pulse waveform and power sequencing through voltage changes and USB attach events before connecting any servo.

## 7. Optional vehicle VIN monitor — RELEASE HOLD
R_VMON_HI 470k and R_VMON_LO 22k divider from protected input to GND_LOGIC; then 1k series / 100nF filter to GPIO6 ADC1_CH5. At nominal 14.4V the ideal divider output is 0.644V. This value is **NOT electrically protected** in the netmap by itself. A correctly dimensioned clamp and transients review must be completed before assembling the circuit. Use eFuse ADC calibration; only diagnostic/undervoltage logging.

## 8. External 24V servo wiring
Vehicle +12 V branch (separate motor-appropriate fuse/wire) -> customer's 240W converter INPUT. Converter +24V -> individual branch protection -> RDS51160 motor positive. Converter 24V negative -> RDS51160 negative. Controller J3.1 -> servo signal. Controller J3.2 -> servo negative near servo. Do **NOT** route converter 24 V or motor current through J3, J4 or PCB. Confirm output 24V under start/light load, near stall and high input voltage; the servo's published absolute normal operating maximum is 25.2 V.

## 9. Electrical isolation caveat
Switches and external 24V boost may reconnect all grounds to the vehicle chassis outside the PCB. The isolation moat prevents PCB local return paths, but doesn't guarantee system-level galvanic isolation. The outputs remain optocoupled because that was a customer requirement; review whether chassis-bonded grounds alter the value/cost of isolation in a later revision.
