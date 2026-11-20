# USB-C bus-power and host presence engineering note (Rev C)

Controller USB-C is planned as **self-powered, data-only**. The USB connector's VBUS must never energize `+5V_LOGIC`, but a conforming self-powered USB device may need to detect host VBUS before attaching the USB pull-up and must prevent back-power through D+/D- while the host is absent.

**CAD review gate:** Decide on a protected VBUS sense input before freezing the schematic and firmware. GPIO7 (ESP32-S3-WROOM-1 module pad 7) is provisionally available. A resistor divider from USB VBUS into GPIO7 with local ESD protection, divider values sized to keep IO below 3.3 V at USB worst-case VBUS, and suitable firmware attach/detach logic are plausible. This circuit has not been selected or verified, and this package's USB drawing does not include it. DO NOT leave USB VBUS connected directly to the main controller rail.

Test case: power controller from nominal vehicle +12 V only, attach/detach USB repeatedly, confirm enumeration and no undesirable D+/D- voltage when host VBUS is absent. Then remove vehicle +12 V with USB still plugged in: there should be **no** power on controller +5 V / 3.3 V rails.
