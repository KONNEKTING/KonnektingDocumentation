# KONNEKTING Device Library

# Example Sketch Explained

```C++
// include KnxDevice library
#include <KnxDevice.h>

// Set the device identification
#define MANUFACTURER_ID 12345
#define DEVICE_ID 123
#define Revision 0

// Set Prog-LED Pin and Prog-Button Pin
#define PROG_LED_PIN LED_BUILTIN  
#define PROG_BUTTON_PIN 3

// Set KNX Transceiver serial port
#ifdef __AVR_ATmega328P__
#define KNX_SERIAL Serial // Nano/ProMini etc. use Serial
#else
#define KNX_SERIAL Serial1 // Leonardo/Micro etc. use Serial1
#endif

// ----------------------------

// Definition of the Communication Objects attached to the device
KnxComObject KnxDevice::_comObjectsList[] = {
    /* the programming com object: Do not remove or push to another index. Must be the first in the list! */
    Tools.createProgComObject(),
    
    /* ComObject ID 0 : Your first com object, referenced in your device XML */ 
    KnxComObject(G_ADDR(0, 0, 1), KNX_DPT_1_001, COM_OBJ_LOGIC_IN),
    
    /* ComObject ID 1 */ 
    KnxComObject(G_ADDR(0, 0, 2), KNX_DPT_1_001, COM_OBJ_SENSOR),
    
    // more com objects if you need ...
};
const byte KnxDevice::_numberOfComObjects = sizeof (_comObjectsList) / sizeof (KnxComObject); // do no change this code

// Definition of parameter size / type ...
// Ensure that this matches your parameter configuration in your device XML
byte KnxTools::_paramSizeList[] = {

    /* Param Index 0 */ PARAM_UINT8,
    /* Param Index 1 */ PARAM_INT16,
    /* Param Index 2 */ PARAM_UINT32,
    
    // more parameters if you need
};
const byte KnxTools::_numberOfParams = sizeof (_paramSizeList); // do no change this code



// Callback function to handle com objects updates
void knxEvents(byte index) {
    
    // index will contain the com-object which received a telegram
    // Do what ever is needed
    
};

void setup() {

    // Initialize KNX enabled Arduino Board
    Tools.init(KNX_SERIAL, 
              PROG_BUTTON_PIN, 
              PROG_LED_PIN, 
              MANUFACTURER_ID, 
              DEVICE_ID, 
              REVISION);
              
    // you can access your parameters by callig
    byte paramId0 = Tools.getUINT8Param(0);
    
    // There's a getXXXXParam method for each known parameter type
}

void loop() {
    // handle KNX stuff
    Knx.task();
}
```

# KONNEKTING Device Arduino-API

TODO add content
