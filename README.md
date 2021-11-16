# ESPHome configuration for the PC-WKS 1167G kettle

This repository includes an ESPHome example configuration for the ProfiCook PC-WKS 1167 G kettle. You can use it to control your kettle as a climate device from within Home Assistant.

## Getting started

1. [Flash tasmota](https://templates.blakadder.com/proficook_PC-WKS_1167.html) to your device

2. Clone this repository and customize it:

    * `git clone https://github.com/Tho85/esphome-wks1167g`
    * Customize your secrets in `includes/secrets.yaml`
    * Customize device name in `wks-1167g.yaml`

3. Compile firmware: `esphome compile wks-1167g.yaml`

4. Flash firmware file in `wks-1167g/.pioenvs/wks-1167g/firmware.bin` via Tasmota web ui

5. Add your device to Home Assistant as usual

## Result

![screenshot](doc/screenshot.png?raw=true)

## Current state

### What works

* `climate` device:
  * Setting custom temperatures
  * Report current temperature
  * Heat to custom temperature (-> on)
  * Send device into standby (-> off)
* `sensor` devices:
  * Current state (kettle removed, heating, cooling, standby, ...)
  * Time remaining
  * Error (`binary_sensor`)
  * Error description

### What doesn't work

* Heating to predefined temperatures / modes available via buttons, e.g. 3 minutes @ 100 °C

  This would require a major rework of the tuya button component, since all modes are set through a single (enum) datapoint. And besides, you can reach any temperature by setting a custom temperature via the `climate` device. Only downside: 3 minutes @ 100°C won't work

## Internals

This example relies on a [patched version of the `tuya` component](https://github.com/Tho85/esphome/tree/Tho85/tuya-WKS1167-hack/esphome/components/tuya). It customizes tuya's climate handling to send the correct enum datapoint for on/off for the device. It also filters out invalid temperature values, where the temperature is read as 100°C when removing the kettle.

The patched version is automatically loaded as a custom component from the example file.

## Copyright

Copyright 2021 Thomas Hollstegge

License: [MIT](LICENSE)
