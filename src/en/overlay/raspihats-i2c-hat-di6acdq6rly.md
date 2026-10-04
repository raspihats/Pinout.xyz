<!--
---
name: DI6acDQ6rly I2C-HAT
class: board
type: io, relay
formfactor: Custom
manufacturer: Raspihats
description: 6 isolated digital inputs with edge counters and 6 relay outputs, stackable over I2C
url: https://raspihats.com/shop/di6acdq6rly-i2c-hat/
github: https://github.com/raspihats/raspihats
buy: https://raspihats.com/shop/di6acdq6rly-i2c-hat/
image: 'raspihats-i2c-hat-di6acdq6rly.png'
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
  '40':
    name: IRQ
    mode: input
    active: low
i2c:
  '0x60-0x6F':
    name: DI6acDQ6rly I2C-HAT
    device: raspihats
-->
# DI6acDQ6rly I2C-HAT

The DI6acDQ6rly I2C-HAT combines 6 opto-isolated digital inputs and 6 power relays on one Raspberry Pi add-on board, controlled over I2C. Each input works with sink or source wiring and switches on anywhere from +3V to +30V, and each relay is a Form A (normally open) contact rated 5A @ 250VAC/30VDC. Every input and relay has an LED indicator.

An onboard microcontroller debounces the inputs and keeps two 32-bit counters per input, one for rising and one for falling edges, so pulses are counted on the board even while the Pi is busy. It also drives the relay coils, holding them with PWM after pull-in to cut average coil power by up to 75%. The relay state applied at power-up (PowerOnValue) is stored on the board, and if the host stops talking to the board for longer than the communication watchdog period, the relays switch to a stored SafetyValue.

The IRQ pin is an open-drain, active-low interrupt that the board pulls when an input selected for interrupts changes, so the Pi doesn't have to poll. Enable the Pi's internal pull-up on GPIO21 to use it. The IRQ line can be moved to another GPIO with solder jumpers.

Up to 16 boards can share one Raspberry Pi: four address jumpers select an I2C address from 0x60 to 0x6F.

## Features

* 6 opto-isolated digital inputs, sink or source, on from +3V to +30V
* Two 32-bit edge counters per input (rising and falling), up to 100Hz, minimum pulse width 5ms
* Interrupt output on input change
* 6 relays, Form A (normally open), 5A @ 250VAC/30VDC
* PWM coil drive, up to 75% lower average power
* Configurable PowerOnValue and SafetyValue, stored on the board
* System and communication watchdogs
* 2000 VAC isolation
* Operating temperature -25 to +75°C
* Stackable, up to 16 boards (I2C addresses 0x60 to 0x6F)
* Mounts on a DIN rail with the [DIN Pi Case](https://raspihats.com/shop/din-pi-case/)

## Example

```python
from raspihats.i2c_hats import DI6acDQ6rly

hat = DI6acDQ6rly(address=0x60)
print(hat.di.value)            # all 6 inputs as a bitmask
print(hat.di.r_counters[0])    # rising edges on input 0
hat.do.power_on_value = 0x00   # all off at power-up
hat.do.channels[0] = True      # energize relay 0
```

## Software

* [Python](https://pypi.org/project/raspihats/) - `pip install raspihats`
* [Node.js](https://www.npmjs.com/package/raspihats)
* [Node-RED](https://www.npmjs.com/package/node-red-contrib-raspihats) - flow-based programming
* [Robot Framework](https://github.com/raspihats/raspihats/blob/master/raspihats/i2c_hats/robot.py) - keyword library for test automation
