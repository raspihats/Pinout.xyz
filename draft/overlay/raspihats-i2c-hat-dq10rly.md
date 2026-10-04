<!--
---
name: DQ10rly I2C-HAT
class: board
type: relay
formfactor: Custom
manufacturer: Raspihats
description: 10 relay outputs with configurable power-on and watchdog safety states, stackable over I2C
url: https://raspihats.com/shop/dq10rly-i2c-hat/
github: https://github.com/raspihats/raspihats
buy: https://raspihats.com/shop/dq10rly-i2c-hat/
image: 'raspihats-i2c-hat-dq10rly.png'
pincount: 40
eeprom: no
power:
  '2':
  '4':
ground:
  '6':
  '9':
  '14':
  '20':
  '25':
  '30':
  '34':
  '39':
pin:
  '3':
    mode: i2c
  '5':
    mode: i2c
i2c:
  '0x50-0x5F':
    name: DQ10rly I2C-HAT
    device: raspihats
-->
# DQ10rly I2C-HAT

The DQ10rly I2C-HAT adds 10 power relays to the Raspberry Pi, controlled over I2C, so it uses no GPIO pins beyond SDA and SCL. Each relay is a Form A (normally open) contact rated 5A @ 250VAC/30VDC, wired to detachable screw terminals, with an LED showing its state.

An onboard microcontroller drives the relay coils. After pull-in it holds them with PWM, which cuts average coil power by up to 75% and keeps a stack of boards within the Pi's 5V budget. The relay state applied at power-up (PowerOnValue) is stored on the board, so relays don't flicker while the Pi boots. If the host stops talking to the board for longer than the communication watchdog period, the outputs switch to a stored SafetyValue.

Up to 16 boards can share one Raspberry Pi: four address jumpers select an I2C address from 0x50 to 0x5F. Raspihats relay boards share this range, so give each one a unique address when mixing them.

## Features

* 10 relays, Form A (normally open), 5A @ 250VAC/30VDC
* PWM coil drive, up to 75% lower average power
* Configurable PowerOnValue and SafetyValue, stored on the board
* System and communication watchdogs
* 2000 VAC isolation
* LED indicator on every relay
* Operating temperature -25 to +75°C
* Stackable, up to 16 boards (I2C addresses 0x50 to 0x5F)
* Mounts on a DIN rail with the [DIN Pi Case](https://raspihats.com/shop/din-pi-case/)

## Example

```python
from raspihats.i2c_hats import DQ10rly

hat = DQ10rly(address=0x50)
hat.do.power_on_value = 0x00  # all off at power-up
hat.do.safety_value = 0x00    # all off if the watchdog fires
hat.do.channels[0] = True     # energize relay 0
```

## Software

* [Python](https://pypi.org/project/raspihats/) - `pip install raspihats`
* [Node.js](https://www.npmjs.com/package/raspihats)
* [Node-RED](https://www.npmjs.com/package/node-red-contrib-raspihats) - flow-based programming
* [Robot Framework](https://github.com/raspihats/raspihats/blob/master/raspihats/i2c_hats/robot.py) - keyword library for test automation
