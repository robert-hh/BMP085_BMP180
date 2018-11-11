# micropython Module for the BMP180 and BMP085 sensors

bmp085.py is a pycom micropython module for the Bosch BMP085/BMP180 sensor.
 It measures temperature as well as pressure, with a high enough resolution to
 calculate altitude.  
Boards: Pycom modules and similar, like ESP8266, generic ESP32 modules. It may be
that the I2C access has to be adapted for non-Pycom modules.

Datasheets:  
https://www.sparkfun.com/datasheets/Components/General/BST-BMP085-DS000-05.pdf  
http://ae-bst.resource.bosch.com/media/products/dokumente/bmp180/BST-BMP180-DS000-09.pdf

This module is based on an 2014 implementation by Sebastian Plamauer,
 oeplse@gmail.com for the esp8266. I have adapted it to the Pycom modules. The
 details of the change are listed in the source code.

## Classes

### bmp = BMP180(i2c=None)
Class for the BMP180 pressure sensor. The I2C bus to be used must be supplied
with i2c.

### bmp = BMP085(i2c=None)
Class for the BMP085 pressure sensor. The I2C bus to be used must be supplied
with i2c.

The two classes differ only by their name.

## Methods

### compval = bmp.compvaldump()
Returns a list of all compensation values.  

### bmp.gauge()
Generator refreshing the measurements. Does not need to be called manually.

### bmp.blocking_read()
Trigger a read and wait until data is available.  

## Properties

### temperature = bmp.temperature
Nonblocking read of temperature in degree C.  

### pressure = bmp.pressure
Nonblocking read of pressure in hPa.  

### altitude = bmp.altitude
Altitude in m. The altitude is calculated based on the value given to
the property sealevel (see below). Obviously, this value does not have to be the
sealevel pressure, but any pressure you may select, for instance to measure
altitude difference in general.

### bmp.sealevel = sealevel
### sealevel = bmp.sealevel
Setting and getting the pressure for altitude calculation.  
The default is 1013.25 hPa, but you can use your local
QNH in hPa, or set a local pressure to determine altitude difference.  

### bmp.oversample = value
### value = bmp.oversample
Setting and getting the oversample setting. The default is  0  

|Mode|Setting|Samples|Conversion Time (ms)|
|:-:|:-:|:-:|:-:|
|low power|0|1|4.5|
|standard|1|2|7.5|
|high res|2|4|13.5|
|ultra high res|3|4|25.5|

## Instance variables

### bmp.chip_id
ID of chip is hard coded on the sensor.


## Example

```python
#
# this example assumes the default connection for the I2C Obviously
# at P9 = sda and P10 = scl
#
from machine import Pin, I2C
from bmp085 import BMP180

i2c = I2C()
bmp = BMP180(i2c)
bmp.oversample = 2
bmp.sealevel = 101325

temp = bmp.temperature
p = bmp.pressure
altitude = bmp.altitude
print(temp, p, altitude)
```

Important Notice:  
Make sure to either use this in fast loops or call gauge() in fast loops to make
sure to always get current values. If you call pressure() once and then again
10 seconds later, it will report a 10 seconds old value. Look at the gauge()
function in the source to understand this. Alternatively use the blocking_read()
method.
