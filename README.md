# Solar Water Level Monitor (ESP8266 + ESPHome)

A solar-powered water tank level monitoring system built around the ESP8266 (ESP-12E), designed in KiCad and running on ESPHome. It measures water level with an ultrasonic sensor, monitors battery current/voltage, and charges itself from a solar panel with a selectable-voltage MPPT charger.

## Features

- **Water level sensing** using an HC-SR04 ultrasonic distance sensor
- **Solar MPPT charging** via CN3791, with jumper-selectable input voltage (6V / 9V / 12V)
- **Battery current & voltage monitoring** using an INA219 sensor
- **1S Li-ion protection** (over-charge / over-discharge / over-current) using DW01A + FS8205A
- **5V boost converter** (MT3608) to power the ESP8266 and sensors from a single Li-ion cell
- **Deep sleep support** for low power consumption between readings
- **ESPHome integration** — no custom firmware needed, works directly with Home Assistant

## Hardware Overview
<img width="4000" height="2666" alt="20260731_152506" src="https://github.com/user-attachments/assets/2ccba99c-d3e1-437a-82a4-d2979a8bdb8b" />
<img width="300" height="200" alt="20260823_192953" src="https://github.com/user-attachments/assets/8ba35884-39f4-4ec2-9a01-51f93df22cbd" />
<img width="300" height="200" alt="20260823_193008" src="https://github.com/user-attachments/assets/843db635-9908-476a-9588-5edd97bc4595" /> 
<img width="300" height="400" alt="20260825_170812" src="https://github.com/user-attachments/assets/fb6d0b2c-f2bd-4c85-8426-e8847a7529d2" />
<img width="300" height="400" alt="20260825_170644" src="https://github.com/user-attachments/assets/0a77fa55-6f49-4a7d-b949-3eeeb73938a8" />
<img width="1281" height="648" alt="Screenshot_20260823_230358" src="https://github.com/user-attachments/assets/f28f9217-f35e-40dd-9a64-fb1d5287de30" />

| Block | Component | Notes |
|---|---|---|
| MCU | ESP-12E (ESP8266) | Runs ESPHome |
| Water sensor | HC-SR04 | 5V supply, echo signal stepped down to 3.3V via voltage divider |
| Current/voltage sensor | INA219 | I2C, monitors battery current (charge & discharge) |
| Boost converter | MT3608 | Steps battery voltage (3.0–4.2V) up to 5V |
| Solar charger | CN3791 | MPPT buck charger, up to 4A, input jumper-selectable for 6V/9V/12V panels |
| Battery protection | DW01A + FS8205A | Standard 1S Li-ion protection circuit |
| Battery | 1x 18650 Li-ion | Not included on the board |

## Pin Connections (ESP-12E)

| Function | GPIO |
|---|---|
| I2C SDA (INA219) | GPIO4 |
| I2C SCL (INA219) | GPIO5 |
| HC-SR04 Trig | GPIO14 |
| HC-SR04 Echo (via divider) | GPIO12 |
| Deep sleep wake | GPIO16 → RST (via solder jumper) |

## Solar Panel Voltage Selection

The CN3791 MPPT tracking voltage is set by closing one solder jumper on the board.
| Panel    | Target MPPT | R18  |         |       VMPPT |
| -------- | ----------: | ---: | ------: | ----------: |
| **6 V**  |      ~5,0 V | 200k | **68k** |  **4,75 V** |
| **9 V**  |      ~8,0 V | 200k | **39k** |  **7,38 V** |
| **12 V** |     ~10,2 V | 200k | **33k** |  **8,50 V** |

> Only close **one** jumper at a time.

## Getting Started

1. Flash the board with [ESPHome](https://esphome.io/), using the provided YAML configuration (`esphome/water-level-sensor.yaml`)
2. Connect the HC-SR04, battery, and solar panel
3. Set the solar jumper according to your panel's voltage
4. Add the device to Home Assistant via the ESPHome integration

## PCB

- Designed in KiCad, 2-layer, 60x60mm
- Gerber files and schematic are in the `/hardware` folder

## License

MIT — feel free to use, modify, and build upon this project.

## Disclaimer

This is a hobbyist/DIY project. Double-check battery polarity and solar jumper settings before powering on. No warranty is provided — build and use at your own risk.
