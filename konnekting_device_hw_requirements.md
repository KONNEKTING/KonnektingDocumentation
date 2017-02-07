KONNEKTING Device Hardware Requirements

The following microcontrollers are supported:
* Atmel ATMEGA 328P, 8Mhz@3.3V / 16Mhz@5V
* Atmel ATMEGA 32u4, 8Mhz@3.3V / 16Mhz@5V
* Atmel SMART SAM D21 (SAMD21G18), 48Mhz@3.3V

Other (Arduino compatible) controllers might work as well. But don't expect that they work flawlessly without adapting our library code.
We try to keep the number of directly supported controllers short.

You need at least two free pins on the microcontroller for 

* Programming Button
* Programming LED

It is required, that the programming button is connected to a pin on the microcontroller which is capable of a real harwdare interrupt. Internally we use the attachInterrupt() Arduino function (https://www.arduino.cc/en/Reference/attachInterrupt). So make sure your button is connected to a pin that is supported by this function.

The pin on the microcontroller for the LED can be any digital output pin.
