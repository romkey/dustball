# The Dustball

** Dustball functionality has been merged into the [Furball](https://github.com/HomeBusProjects/furball) codebase. No further work will happen with this repo **

Dustball is an open-source hardware project based on Furball, which is an open-source hardware project intended to help with the research and development of HomeBus, an IoT auto-provisioning framework.

HomeBus is nowhere near ready to go and we want to use Dustball now, so Dustball will start out reporting via a REST API to the Dustball server.

The name 'Furball' is an homage to Dave Mills' ["Fuzzball"](https://en.wikipedia.org/wiki/Fuzzball_router), one of the first routers on the nascent Internet.

The Furball hardware performs environmental monitoring and are intended to be deployed indoors with fixed power source. Its Dustball variant is intended for indoor or outdoor use, given a suitable case to protect it from the elements.

## Current Status

Dustball reboots frequently. It is strongly dependent on a stable, reliable power supply. Without one it may hang frequently.

Dustball works with Adafruit and Sparkfun CCS811 sensors, but not the cheaper CCS811 boards that I bought in bulk from AliExpress. I suspect an intialization problem but haven't had time to debug it.

## Hardware

Furball and Dustball are based on the ESP32 processor. ESP32 is more capable than its predecessor, the ESP8266 while remaining inexpensive. The ESP32 supports both 802.11b/g/n Wifi and Bluetooth 4.2/BLE. It also has hardware acceleration for encryption. It includes 4 SPI controllers, 2 I2C controllers, 3 UARTs, up to 18 channels of ADC and two 8 bit DACs. 

The hardware should support several common environmental sensors in order of importance:
- particle counter
- temperature
- humidity
- air pressure 
- light
- volatile organics

It shoud be powered via USB.

Its USB port should be accessible externally for programming in addition to power.

It should include a tri-color LED for debugging and identification purposes.

### Temperature, Humidity and Pressure

The BME280 - temperature/pressure/humidity. $2.57 in single unit quantities, AliExpress.

### Light

The TSL2561  Digital Luminosity sensor - $1 in single unit quantities, AliExpress.

### VOC

The CCS811, VOC and eCO2 sensor. $9 in single unit quantities, AliExpress. Some listings confuse CO2 and CO. This sensor computes a derived Carbon Dioxide value - anything that says it can measure Carbon Monoxide is incorrect.

### Particle Count

[Plantower PMS5003](https://www.adafruit.com/product/3686) - $39, Adafruit.

### Total Cost

If bought through AliExpress, parts cost should run roughly:
- $6 - ESP32 LOLIN32
- $2.57 - BME280
- $1 - TSL2561
- $9 - CCS811
- $1 - random resistors, capacitors, tri-color LED
- $40 - PMS5003

Total of roughly $20 in parts before the circuit board.

## Building The Hardware

I'm still working on a schematic, but the hardware is very simple to build.

Connect 3.3V power and ground from the ESP32 to each of the BME280, TSL2561 and CCS811. Connect 5V and ground from the ESP32 to the PMS5003 (the voltage levels on the PMS5003 will be fine - it needs 5V but it drives its data lines at 3.3V).

Connect the I2C SDA and SDC lines from the ESP32 to the BME280, TSL2561 and CCS811. Also connect SDA and SDC each via a 470K resistor to 3.3V.  SDA and SDC are run open-collector and need pull up resistors.

Connect the PMS5003's RX and TX wires to the ESP32.

Install a large (100µF) capacitor between 3.3V and ground, as phyiscally close to the ESP32 as possible. Also install a small (100pF) capacitor between 3.3V and ground as physically close to each of the BME280, TSL2561, CCS811 and PMS5003. These will help stabilize power to each device.

Try wiring up the I2C devices each one at a time and testing each time to make sure they work. If you run into problems, try using the ESP32 diagnostic firmware to do an I2C scan - it's possible your I2C board might be using a different address than the software defaults to.

## Software

Dustball's firmware uses the Arduino SDK for ESP32 and is organized to build using PlatformIO, which runs under Linux, MacOS and Windows.

The software provides a uniform, abstract interface to each device using the `Sensor` class. The `Sensor` class provides some housekeeping functions to help the software poll the devices at the appropriate intervals.

# License

Software and documentation are licensed under the [MIT license](https://romkey.mit-license.org/).

Hardware designs are licensed under [Creative Commons Attribution Share-Alike license](https://creativecommons.org/licenses/by-sa/4.0). 
