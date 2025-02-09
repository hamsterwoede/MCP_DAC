
[![Arduino CI](https://github.com/RobTillaart/MCP_DAC/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/MCP_DAC/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/MCP_DAC.svg?maxAge=3600)](https://github.com/RobTillaart/MCP_DAC/releases)


# MCP_DAC

Arduino library for Microchip SPI DAC's:  8, 10, 12 bits, 1, 2 channel


## Description

The MCP_DAC is a library for DAC's from Microchip in the MCP48xx en MCP49xx series.
The library is experimental as it is not tested with all different devices.
Please post an issue if there are problems.


|  Type   | Channels | Bits | MaxValue | Voltage reference |
|:--------|:--------:|:----:|:--------:|:-----------------:|
| MCP4801 |  1       |  8   |   255    | internal 2.048 V  |
| MCP4802 |  2       |  8   |   255    | internal 2.048 V  |
| MCP4811 |  1       |  10  |   1023   | internal 2.048 V  |
| MCP4812 |  2       |  10  |   1023   | internal 2.048 V  |
| MCP4821 |  1       |  12  |   4095   | internal 2.048 V  |
| MCP4822 |  2       |  12  |   4095   | internal 2.048 V  |
| MCP4901 |  1       |  8   |   255    | external          |
| MCP4902 |  2       |  8   |   255    | external          |
| MCP4911 |  1       |  10  |   1023   | external          |
| MCP4912 |  2       |  10  |   1023   | external          |
| MCP4921 |  1       |  12  |   4095   | external          |
| MCP4922 |  2       |  12  |   4095   | external          |


The output voltage of the MCP_DAC depends on the voltage supplied, 
which is in the range of 2.7V .. 5.5V. Check datasheet for the details.


## Interface


### Constructor

- **MCP_DAC(uint8_t dataOut = 255, uint8_t clock = 255)** Constructor. 
  Other datatypes idem.
- **begin(uint8_t select, uint8_t latchPin = 255)** defines the select pin. The select pin is for device selection in case of multiple SPI devices. 
- **uint8_t channels()** returns the number of channels.
- **uint16_t maxValue()** returns the maximum value. relates to # bits, see table above.


### Gain

- **bool setGain(uint8_t gain)** gain is 1 or 2.
- **uint8_t getGain()** returns gain set, default 1.

The analog output cannot go beyond the supply voltage. 
So if Vref is connected to 5V, gain will not make 10 of it.


### Write

- **bool analogWrite(uint16_t value, uint8_t channel = 0)** writes value to channel. The value is limited to maxValue. Returns false on an invalid channel.
- **uint16_t lastValue(uint8_t channel = 0)** returns last written value, default for channel 0 as that works for the single DAC devices.
- **void setPercentage(float perc)** perc = 0..100.0%  Wrapper around **analogWrite()**.
- **float getPercentage(uint8_t channel = 0)** returns percentage. Wrapper around **lastValue()**.
- **void fastWriteA(uint16_t value)** faster version to write to channel 0. Does not check flags and does not update **lastValue()**
- **void fastWriteB(uint16_t value)** faster version to write to channel 1. Does not check flags and does not update **lastValue()**

For fastest speed there is an example added **MCP4921_standalone.ino**. 
That squeezes the most performance out of it for now.
Code for the other MCP4xxx can be written in same way.


### Shutdown

- **void shutDown()** shuts down the device, optional one might need to **triggerLatch()**
- ** bool isActive()** returns false if device is in shutdown mode.


### Hardware SPI

To be used only if one needs a specific speed.

- **void setSPIspeed(uint32_t speed)** set SPI transfer rate
- **uint32_t getSPIspeed()** returns SPI transfer rate


### LDAC

- **void setLatchPin(uint8_t latchPin)** defines the latchPin, this is optional. The latchPin is used for simultaneous setting a value in multiple devices. Note the latchPin must be the same for all instances that need to be triggered together.
- **triggerLatchPin()** toggles the defined latchPin, and all devices that are connected to it.


### Buffered 

MCP49xxx series only, see page 20 ==> not functional for MCP48xx series.

- **void setBufferedMode(bool mode)** set buffered mode on/off. The default = false, unbuffered.
- **bool getBufferedMode()** returns set value


## Future

- test test test and ....
- 



## Operation

See examples
