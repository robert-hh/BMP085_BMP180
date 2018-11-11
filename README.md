micropython-bmp180 and BMP085
==================

This module is based on an 2014 implementation by 2014 Sebastian Plamauer,
 oeplse@gmail.com for the esp8266. I have adapted it to the Pycom modules.


Module bmp085 and bmp180
-----------------
bmp085/bmp180 is a pycom micropython module for the Bosch BMP085/BMP180 sensor.
 It measures temperature as well as pressure, with a high enough resolution to
 calculate
altitude.  
Breakoutboard: http://www.adafruit.com/products/1603  
data-sheet: http://ae-bst.resource.bosch.com/media/products/dokumente/bmp180/BST-BMP180-DS000-09.pdf

If you have any questions, open an issue.

### Quickstart

Example:
```python
from machine import Pin, I2C
from bmp180 import BMP180

i2c = I2C()
bmp180 = BMP180(i2c)
bmp180.oversample = 2
bmp180.sealevel = 101325

temp = bmp180.temperature
p = bmp180.pressure
altitude = bmp180.altitude
print(temp, p, altitude)
```

Important Notice:  
Make sure to either use this in fast loops or call gauge() in fast loops to make
sure to always get current values. If you call pressure() once and then again
10 seconds later, it will report a 10 seconds old value. Look at the gauge()
function in the source to understand this. Alternatively use the blocking_read()
method.

Classes
-------
``BMP180``  
Module for the BMP180 pressure sensor.  

``BMP085``  
Module for the BMP085 pressure sensor.  

The classes differ only by their name-

Methods
--------------

``compvaldump()``
Returns a list of all compensation values.  

``gauge()``  
Generator refreshing the measurements. Does not need to be called manually.

``temperature``  
Nonblocking read of temperature in degree C.  

``pressure``  
Nonblocking read of pressure in hPa.  

``blocking_read()``  
Trigger a read and wait until data is available.  

``altitude``  
Altitude in m.  


Instance variables
------------------
``chip_id``  
ID of chip is hardcoded on the sensor.

``oversample``  
Sets the accuracy. Default: 0  
* 0 lowest accuracy, fastest
* 1
* 2
* 3 highest accuracy, slowest

``sealevel``  
Pressure at Main Sea Level. The default is 101325 Pa, but you can use your local
QNH in Pa.  
