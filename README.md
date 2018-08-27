# The Dustball

Dustball is an open-source hardware project based on Furball, which is
an open-source hardware project intended to help with the research and development of HomeBus,
 an IoT auto-provisioning framework.

The name 'Furball' is an homage to Dave Mills' ["Fuzzball"](https://en.wikipedia.org/wiki/Fuzzball_router), one of the first routers on the nascent Internet.

The Furball hardware performs environmental monitoring and is intended to be deployed indoors with fixed power source. It will auto-provision with HomeBus to feed sensor readings into an IoT system.

## Hardware

Furball is based on the ESP32 processor. ESP32 is more capable than its predecessor, the ESP8266 while remaining inexpensive. The ESP32 supports both 802.11b/g/n Wifi and Bluetooth 4.2/BLE. It also has hardware acceleration for encryption. It includes 4 SPI controllers, 2 I2C controllers, 3 UARTs, up to 18 channels of ADC and two 8 bit DACs. 

The hardware should support several common environmental sensors in order of importance:
- temperature
- humidity
- air pressure 
- light
- volatile organics
- particle count

It should be able to measure and report its own input voltage.

It should express one of its I2C ports to an external optional connector along with power (5V/3.3V/GND).

It might also express one of its SPI ports to an external optional connector (5V/3.3V/GND)..

It shoud be powered via USB.

Its USB port should be accessible externally for programming in addition to power.

It should include a tri-color LED for debugging and identification purposes.

### Temperature, Humidity and Pressure

Consider BME280 - temperature/pressure/humidity. $2.57 in single unit quantities, AliExpress.


### Light

Consider TSL2561  Digital Luminosity sensor - $1 in single unit quantities, AliExpress.

### VOC

Consider CCS811, VOC and CO2 sensor. $9 in single unit quantities, AliExpress.

### Particle Count

[Plantower PMS5003](https://www.adafruit.com/product/3686)


### Total Cost

If bought through AliExpress, parts cost should run roughly:
- $6 - ESP32 LOLIN32
- $2.57 - BME280
- $1 - TSL2561
- $9 - CCS811
- $1 - random resistors, capacitors, tri-color LED
- 

Total of roughly $20 in parts before the circuit board.

### Thoughts

I2C is unstable when too many things are connected to it. Given that the ESP32 has two I2C controllers, one should be dedicated for add-on boards and the other should avoid having too many items connected to it. So try to use SPI when possible. We really want this thing to be simple and stable.

## Software

For simplicity, Furball will run the ESP32 Arduino SDK using HomeBus to connect to and pubish to an MQTT server.

Software should allow an update to be pushed locally over wifi or pulled automatically.

# License

Software and documentation is licensed under the [MIT license](https://romkey.mit-license.org/).

Circuits are licensed under [Creative Commons Attribution Share-Alike license](https://creativecommons.org/licenses/by-sa/4.0). 
