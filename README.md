# MicroFoundry-XC6194A-WS2811-Expansion-PCB
Information about the XC6194A Expansion PCB based on the WS2811 IC to drive 3 LEDs and discrete logic to provide voltage translation

## Description
The Micro Foundry XC6194A WS2811 (aka: NeoPixel) RGB Expansion PCB provides the ability to drive a RGB LED illuminated Momentary switch or up to 3 individual LED indicators (with common anode) in addition to offering voltage translation where the XC6194A switched VCC differs from MCU VCC. 

The WS2811 IC provides 256 levels of PWM control per channel and a single wire NZR communication protocol supported by numerous Aurdino libraries such as:
- [Adafruit NeoPixel](https://github.com/adafruit/Adafruit_NeoPixel)
- [NeoPixelBus by Makuna](https://github.com/Makuna/NeoPixelBus/wiki)
- [FastLED](https://github.com/FastLED/FastLED)
- And many others can be found within the Aurduino Library Manager...

**Please NOTE:** Color representation is totally dependant on the LEDs utilized. Therefore there are no guarentees this LED driver will have the ability to produce the accurite color representation possible with 3x LEDs.

## Features
- 3 Channels of LED switching with 18mA constant current sink capability per channel (@ 5v)
- An additional "Power State" channel to provide a sink to LED channel 1 when XC6194 is in Off State (more details [below](/README.md#power-state-channel))
- Voltage translation on SW (Switch) output -> MCU input with Schmitt-Trigger input/inverted output
- Anode series resistor to provide global LED current control (more details [below](/README.md#anode-series-resistor))
- On-board micro switch for secondary XC6194A control
- Mount holes will accept Wurth Electronics SMT Standoffs

## Pinouts
### Switch

| Pin | Function | Description | Voltage Potential |
| --- | -------- | ----------- | ---------- |
| 1 | SW | Momentary Switch Input (Active LOW) | XC6194A VIn |
| 2 | GND | Ground | |
| 3 | ANN |Common LED Anode | XC6194A VIn |
| 4 | LED1 | LED 1 Cathode | |
| 5 | LED2 | LED 2 Cathode | |
| 6 | LED3 | LED 3 Cathode | |

### IO (to MCU)

| Pin | Function | Description | MCU IO | Voltage Potential |
| --- | -------- | ----------- | ------ | ---------- |
| 1 | GND | Ground | | GND |
| 2 | VCC | Voltage In | | MCU VCC |
| 3 | SW | Switch Pressed | Input (Active HIGH) | MCU VCC |
| 4 | SHDN | Shutdown | Output (Active HIGH) | MCU VCC |
| 5 | PG | Power Good | Output (Active HIGH) **See NOTE-1** | XC6194A VIn |
| 6 | DI | WS2811 Data In | Input | MCU VCC |
| 7 | DO | LED 2 Driver | Output | MCU VCC |
| 8 | NC | No connection to PCB |  |  |

**NOTE-1:** The Power Good Output is traditionally uitilized to enable a downstream power supply, therefore the output operates at XC6194A VIn Potential. Please verify voltage compatability before connecting.

### Expansion

| Pin | Function | Description | XC6194A IO | Voltage Potential |
| --- | -------- | ----------- | ---------- | ---------- |
| 1 | VIn | XC6194A Switched Voltage In | | XC6194A VIn |
| 2 | GND | Ground | | GND |
| 3 | VOut | XC6194A Switched Voltage Out | | XC6194A VOut |
| 4 | SW | XC6194A Momentary Switch | Input (Active LOW) | XC6194A VIn |
| 5 | PG | XC6194A Power Good | Output (Active HIGH) | XC6194A VIn |
| 6 | SHDN | XC6194A Shutdown | Input (Active HIGH) **See NOTE-2** | GND |

**NOTE-2:** Circuit is pulled to GND via pull-down resistor. XC6149A datasheet specifies a voltage level above 1.1v will trigger shutdown. Some MCUs may exihibit an output glitch on the SHDN during power-up and the XC6194 may immediately shutdown. If this is experienced, a user implemented RC filter could possibly eliminate the issue.

## Power State Channel
Channel 1 LED has a SPDT analog switch with its input referenced to VOut. When the XC6194A is in its off-state, the switch will connect the Channel 1 LED cathode to GND, therefore illuminating the LED. This can be useful to provide a vsual indicator that the power is off. When the XC6194A is in its on-state, the switch will connect the Channel 1 LED cathode to the WS2811 which will be free to drive the LED. If there is no desire to have the Channel 1 LED illuminated when the power is off, this feature can disabled by relocating the R1/R2 resistor as indicated in the following image:

**TODO:** Add image to indicate R1/R2 relocation...

## Anode Series Resistor
The resistor noted in the following image offers global current control via the common anode pin. The default value as manufactured is 0 Ohms (Zero) and can be replaced if necessary.

**TODO:** Add image to indicate R3 location...
