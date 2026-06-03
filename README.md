# PM Sensor ESPHome Project

A lightweight ESPHome project for an ESP32-based particulate matter (PM) sensor module. The device reads PMSx003 data over UART, publishes PM1.0 / PM2.5 / PM10 sensor values, and renders PM2.5 / PM10 information with a simple AQI-style status message on an SH1106 OLED display.

## Features

- ESP32 with ESP-IDF framework
- UART-connected PMSx003 particle sensor
- `PMSX003` sensor platform for PM1.0, PM2.5, and PM10 readings
- OLED display over I2C (`SH1106 128x64`) showing:
  - PM2.5 value
  - PM10 value
  - AQI-style status text
- Home Assistant API support
- OTA updates
- Wi-Fi credentials loaded from `secrets.yaml`

## Hardware

- ESP32 board
- PMSx003 or compatible particulate matter sensor. I use a PMS7003, but the configuration should work with other PMSx003 variants.
- SH1106 128x64 OLED display
- I2C wiring:
  - `SDA` -> `GPIO21`
  - `SCL` -> `GPIO22`
- UART wiring for PMSx003:
  - `RX` -> `GPIO16`
  - `TX` -> `GPIO17`

*Wiring may vary based on your specific ESP32 board and sensor module. Adjust GPIO pins in the YAML configuration as needed.*

## Configuration

Update the following values in `secrets.yaml`:

```yaml
wifi_ssid: "your_wifi_ssid"
wifi_password: "your_wifi_password"
```

If required, adjust the display address, I2C pins, UART pins, or sensor update interval inside `esphome-web-e81744.yaml`.

## Flashing

Use the ESPHome CLI to compile and upload the firmware to the ESP32:

```bash
esphome run esphome-web-e81744.yaml
```

For OTA updates after the first flash, run:

```bash
esphome upload esphome-web-e81744.yaml
```

## Home Assistant

This project enables the ESPHome API, so the device can be integrated directly with Home Assistant. Once the device is online, Home Assistant should discover it automatically if API discovery is enabled.

Sensor entities exposed:

- `Particulate Matter <1.0µm Concentration`
- `Particulate Matter <2.5µm Concentration`
- `Particulate Matter <10.0µm Concentration`

## Display Behavior

The OLED display shows:

- PM2.5 on the first row
- PM10 on the second row
- AQI-style status on the third row:
  - `GOOD :)` when PM2.5 ≤ 12
  - `MODERATE` when PM2.5 ≤ 35
  - `BAD !` when PM2.5 > 35

## Notes

- The `font_small` font uses Google Fonts Roboto.
- The current YAML uses `min_version: 2026.3.0` for ESPHome compatibility.

## License

Feel free to modify and reuse this configuration for your own ESPHome PM sensor project.
