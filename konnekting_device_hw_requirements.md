#KONNEKTING Device Hardware Requirements

The following microcontrollers are supported:

* [Atmel ATMEGA 328P, 8Mhz@3.3V / 16Mhz@5V](http://www.atmel.com/devices/ATMEGA328P.aspx)
* [Atmel ATMEGA 32u4, 8Mhz@3.3V / 16Mhz@5V](http://www.atmel.com/devices/atmega32u4.aspx)
* [Atmel SMART SAM D21 (SAMD21G18), 48Mhz@3.3V](http://www.atmel.com/products/microcontrollers/ARM/SAM-D.aspx#samd21)

Other (Arduino compatible) controllers might work as well. But don't expect that they work flawlessly without adapting our library code.
We try to keep the number of directly supported controllers short, so that the overall maintenance is easy. We don't want to add dozens of if-else-compiler-flags to support each and every available microcontroller. You can do this by yourself. But please accept that this will not be merged in the public code (maintenance ...)

You need at least two free pins on the microcontroller for 

* Programming Button
* Programming LED

It is required, that the programming button is connected to a pin on the microcontroller which is capable of a real harwdare interrupt. Internally we use the attachInterrupt() Arduino function (https://www.arduino.cc/en/Reference/attachInterrupt). So make sure your button is connected to a pin that is supported by this function.

The pin on the microcontroller for the LED can be any digital output pin.


