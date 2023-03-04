# HD44780 Character LCD Support for MMDVMHost #

Support for the **HD44780 LCD** is via the **wiringPi** library available at
http://wiringpi.com/download-and-install/ which must be installed in all cases.

The HD44780 in _4-bit mode_ is probably the most common connection method and
wiring details can be found at http://wiringpi.com/dev-lib/lcd-library/

The setup that is commonly used for MMDVM is the 4-bit connection that is also
documented within wiringPi. The HD44780 displays have a standardized 16-pin
connector _(14 pins if without backlight)_. The pin numbers on HD44780 refer to
this standard numbering.

The pin numbers on Raspberry Pi side are the physical pin numbers of the GPIO
pin header, in brackets the wiringPi pin number as these are referred to in the
`MMDVM.ini` file.


## HD44780 Raspberry Pi Wiring Diagram ##

The wiring is as follows:

```
 HD44780 pin    Raspberry Pi GPIO
  GND  1  --------   6 GND
  VCC  2  --------   2 +5V
  V0   3  --------     Trimmer between +5V and GND for contrast
  RS   4  --------  26 CE1 (11)
  RW   5  --------   6 GND
  E    6  --------  24 CE0 (10)
  D0   7
  D1   8
  D2   9
  D3  10
  D4  11  --------  11 GPIO0 (0)
  D5  12  --------  12 GPIO1 (1)
  D6  13  --------  13 GPIO2 (2)
  D7  14  --------  15 GPIO3 (3)
  VCC 15  --------   2 +5V
  GND 16  --------   6 GND
```


## Configuration in `MMDVM.ini` ##

The relevant part in the `MMDVM.ini` is like outlined below:

```
[HD44780]
Rows=4
Columns=20

# For basic HD44780 displays (4-bit connection)
# rs, strb, d0, d1, d2, d3
Pins=11,10,0,1,2,3
```


## Compilation Instructions ##

To compile MMDVMHost with support for the HD44780 use the following commands:

```
$ make clean
$ make -f Makefile.Pi.HD44780
```


## I2C Connections ##

Other HD44780 variations exist that connect via the **I2C** bus. Support is also
via wiringPi, but to compile for each I2C device requires a different `Makefile`
depending on the _GPIO expander IC_ attached to the LCD.


### Adafruit RGB LCD Shield ###

The Adafruit RGB LCD Shield is available from https://www.adafruit.com/product/714

I2C is via the [Microchip][3] **MCP23017** IC and can be compiled with the
`Makefile.Pi.Adafruit` file. The I2C device address in `MMDVM.ini` should be set to `0x20`.


### PCF8574 IC ###

HD44780 LCDs connected via a PCF8574 Remote 8-Bit I/O Expander are available on
[eBay][1] and [Amazon][2] for very little money!

This IC uses Makefile.Pi.PCF8574 and the I2C device address should be set to `0x27`.


### BEWARE ###

The I2C device address can be manually configured on some devices! 


#### Check Your Actual Device Address ####

More information on configuring and using I2C on the RPi including how to probe
the I2C bus for device addresses can be found at:
https://learn.sparkfun.com/tutorials/raspberry-pi-spi-and-i2c-tutorial#i2c-on-pi


## PWM Backlight Control ##

Whilst connected in _4-bit mode_ or via the _PCF8574 IC_, the LED backlight can be
wired to have its brightness controllable via PWM. Connect the backlight **+ve** pin
to any spare GPIO pin and configure `MMDVM.ini` as appropriate. By default, wiringPi
**pin 21** is configured.




[1]:	<https://www.ebay.com>
[2]:	<https://www.amazon.com>
[3]:	<https://www.microchip.com>
