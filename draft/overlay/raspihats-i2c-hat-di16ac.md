<!--
---
name: DI16ac I2C-HAT
class: board
type: io
formfactor: Custom
manufacturer: Raspihats
description: 16 isolated digital inputs with 32-bit edge counters, stackable over I2C
url: https://raspihats.com/shop/di16ac-i2c-hat/
github: https://github.com/raspihats/raspihats
buy: https://raspihats.com/shop/di16ac-i2c-hat/
image: 'raspihats-i2c-hat-di16ac.png'
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
  '0x40-0x4F':
    name: DI16ac I2C-HAT
    device: raspihats
-->
# DI16ac I2C-HAT

The DI16ac I2C-HAT adds 16 opto-isolated digital inputs to the Raspberry Pi. Each input works with sink or source wiring and switches on anywhere from +3V to +30V, so it reads 24V industrial sensors and PLC outputs as well as dry contacts and 3.3V/5V logic. The inputs are split into four blocks of four channels, each block with its own common pin, on two detachable 3.81mm screw terminals.

An onboard microcontroller debounces every input and keeps two 32-bit counters per channel, one for rising and one for falling edges. Pulses are counted on the board even while the Pi is busy, and the host reads the totals over I2C.

The IRQ pin is an open-drain, active-low interrupt that the board pulls when an input selected for interrupts changes, so the Pi doesn't have to poll. Enable the Pi's internal pull-up on GPIO21 to use it. The IRQ line can be moved to another GPIO with solder jumpers.

Up to 16 boards can share one Raspberry Pi: four address jumpers select an I2C address from 0x40 to 0x4F.

## Features

* 16 opto-isolated digital inputs, sink or source, on from +3V to +30V
* 2000 VAC isolation
* Two 32-bit edge counters per input (rising and falling), up to 100Hz, minimum pulse width 5ms
* Interrupt output on input change
* System and communication watchdogs
* LED indicator on every input
* Operating temperature -25 to +75°C
* Stackable, up to 16 boards (I2C addresses 0x40 to 0x4F)
* Mounts on a DIN rail with the [DIN Pi Case](https://raspihats.com/shop/din-pi-case/)

## Example

```python
from raspihats.i2c_hats import DI16ac

hat = DI16ac(address=0x40)
print(hat.di.value)          # all 16 inputs as a bitmask
print(hat.di.r_counters[0])  # rising edges on input 0
```

## Software

* [Python](https://pypi.org/project/raspihats/) - `pip install raspihats`
* [Node.js](https://www.npmjs.com/package/raspihats)
* [Node-RED](https://www.npmjs.com/package/node-red-contrib-raspihats) - flow-based programming
* [Robot Framework](https://github.com/raspihats/raspihats/blob/master/raspihats/i2c_hats/robot.py) - keyword library for test automation
