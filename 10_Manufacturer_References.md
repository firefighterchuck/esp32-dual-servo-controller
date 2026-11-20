# Component specifications and design sources checked 2026-11-20

- DSSERVO RDS51160 operating 19.2-25.2V, 4.2A stall @24V, 500-2500us position pulses, 50-330 Hz: https://aifitlab.com/collections/dsservo-motor/products/dsservo-rds51160-servo-motor
- ESP32-S3 WROOM-1 datasheet module-pad table pp. 10-12: https://www.espressif.com/sites/default/files/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_en.pdf
- Espressif ESP32-S3 hardware reference, 3.3V rail >=500mA, reset 10k/1uF, USB19/20: https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32s3/schematic-checklist.html
- Vishay 6N137 pins 2/3/5/6/7/8, output open drain: https://www.vishay.com/docs/84732/6n137_vo2601_vo2611.pdf
- Nexperia 74AHCT1G14GW pin 2=A, 3=GND, 4=Y, 5=Vcc, 1=NC: https://assets.nexperia.com/documents/data-sheet/74AHC_AHCT1G14.pdf
- MORNSUN IB0505XT-W75R3 datasheet pp. 1,3, 150/15mA output range, pinout 1/2/4/5/8 and caps input 4.7uF, output 10uF: https://www.mornsun-power.com/public/uploads/pdf/IB05_XT-W75R3.pdf
- Texas Instruments LM5164-Q1 rated automotive input 6-100V and 1A output: https://www.ti.com/product/LM5164-Q1
- LM5164-Q1 official datasheet (5V variant design required, EVM outputs 12V by default): https://www.ti.com/document-viewer/LM5164-Q1/datasheet
- LM5164-Q1EVM: https://www.ti.com/tool/LM5164-Q1EVM
- TI TPS62162-Q1 fixed 3.3V 1A buck: https://www.ti.com/product/TPS62162-Q1
- TPS62162-Q1 fixed feedback wiring: https://www.ti.com/lit/ds/symlink/tps62162-q1.pdf
- PCBWay assembly upload requirements: https://www.pcbway.com/assembly-file-requirements.html

Quoted manufacturer values may vary with variant or later revision; confirm all MPNs and drawings at final schematic sign-off.
