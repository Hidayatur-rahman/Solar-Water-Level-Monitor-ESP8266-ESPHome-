# Solar Water Level Monitor (ESP8266 + ESPHome)

A solar-powered water tank level monitoring system built around the ESP8266 (ESP-12E), designed in KiCad and running on ESPHome. It measures water level with an ultrasonic sensor, monitors battery current/voltage, and charges itself from a solar panel with a selectable-voltage MPPT charger.

## Features

- **Water level sensing** using an HC-SR04 ultrasonic distance sensor
- **Solar MPPT charging** via CN3791, with jumper-selectable input voltage (6V / 9V / 12V / 18V)
- **Battery current & voltage monitoring** using an INA219 sensor
- **1S Li-ion protection** (over-charge / over-discharge / over-current) using DW01A + FS8205A
- **5V boost converter** (MT3608) to power the ESP8266 and sensors from a single Li-ion cell
- **Deep sleep support** for low power consumption between readings
- **ESPHome integration** — no custom firmware needed, works directly with Home Assistant

## Hardware Overview
<img width="500" height="375" alt="20260731_152506" src="https://github.com/user-attachments/assets/9a2e9e80-932f-4eb9-9aea-858a7e09985a" /> <img width="500" height="375" alt="20260823_192953" src="https://github.com/user-attachments/assets/2a5e3974-9380-4a58-840c-a5de39bf9059" />




| Block | Component | Notes |
|---|---|---|
| MCU | ESP-12E (ESP8266) | Runs ESPHome |
| Water sensor | HC-SR04 | 5V supply, echo signal stepped down to 3.3V via voltage divider |
| Current/voltage sensor | INA219 | I2C, monitors battery current (charge & discharge) |
| Boost converter | MT3608 | Steps battery voltage (3.0–4.2V) up to 5V |
| Solar charger | CN3791 | MPPT buck charger, up to 4A, input jumper-selectable for 6V/9V/12V/18V panels |
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

The CN3791 MPPT tracking voltage is set by closing one solder jumper on the board. Leave all jumpers open for the 6V default.

| Panel voltage | Jumper to close |
|---|---|
| 6V (default) | none |
| 9V | JP (9V) |
| 12V | JP (12V) |
| 18V | JP (18V) |

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
