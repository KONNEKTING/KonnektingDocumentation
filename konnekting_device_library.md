# KONNEKTING Device Library

The device library enables the Arduino to connect to the KNX bus. 

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
    
    /* ComObject ID 0 : Your first com object, referenced in your device XML */ 
    KnxComObject(KNX_DPT_1_001, COM_OBJ_LOGIC_IN),
    
    /* ComObject ID 1 */ 
    KnxComObject(KNX_DPT_1_001, COM_OBJ_SENSOR),
    
    // more com objects if you need ...
};
const byte KnxDevice::_numberOfComObjects = sizeof (_comObjectsList) / sizeof (KnxComObject); // do no change this code

// Definition of parameter size / type ...
// Ensure that this matches your parameter configuration in your device XML
byte KonnektingDevice::_paramSizeList[] = {

    /* Param Index 0 */ PARAM_UINT8,
    /* Param Index 1 */ PARAM_INT16,
    /* Param Index 2 */ PARAM_UINT32,
    
    // more parameters if you need
};
const byte KonnektingDevice::_numberOfParams = sizeof (_paramSizeList); // do no change this code



// Callback function to handle com objects updates
void knxEvents(byte index) {
    
    // index will contain the com-object which received a telegram
    // Do what ever is needed
    
};

void setup() {

    // Initialize KNX enabled Arduino Board
    Konnekting.init(KNX_SERIAL, 
              PROG_BUTTON_PIN, 
              PROG_LED_PIN, 
              MANUFACTURER_ID, 
              DEVICE_ID, 
              REVISION);
              
    if (!Konnekting.isFactorySetting()) {
        
        // you can access your parameters by callig
        byte paramId0 = Konnekting.getUINT8Param(0);
    
        // There's a getXXXXParam method for each known parameter type
    }
}

void loop() {
    // handle KNX stuff
    Knx.task();
    if (Konnekting.isReadyForApplication()) {
        // do device related stuff
    }
}
```

# KONNEKTING Device Arduino-API

## API
### 1/ Define the communication objects
First of all, define the KNX communication objects of your bus device. For each object, define its datapoint type, and its flags. Theoritically, you can define up to 255 objects, even if in practical you are limited by the quantity of RAM (it would be worth measuring the max allowed number of objects depending on the memory available).

**`KnxComObject KnxDevice::_comObjectsList[];`**

* **Description:** list of the communication objects (group objects) that are attached to your KNX device. Define this variable in your Arduino sketch (but outside all function bodies).
* **Parameters:** for each object in the list, you shall provide the group address (word, use G_ADDR() function), the datapoint type (check "_e_KnxDPT_ID_" enum in [KnxDPT.h](https://github.com/franckmarini/KnxDevice/blob/master/KnxDPT.h) file), and the flags (byte, check [KnxComObject.h](https://github.com/franckmarini/KnxDevice/blob/master/KnxComObject.h) for more details). 
* **Example:** 
```

//           DataPointType						                	flags			
KnxComObject{KNX_DPT_1_001 /* 1.001 B1 DPT_Switch */ ,	          COM_OBJ_LOGIC_IN_INIT	} 
KnxComObject{KNX_DPT_5_010 /* 5.010 U8 DPT_Value_1_Ucount */ ,	  COM_OBJ_SENSOR		} 
KnxComObject{KNX_DPT_1_003 /* 1.003 B1 DPT_Enable*/ ,		      0x30 /* C+R */		}
```
___
**`const byte KnxDevice::_comObjectsNb = sizeof(_comObjectsList) / sizeof(KnxComObject);`**
* **Description:** Define the number of group objects in the list. Simply copy the above code as is in your Arduino sketch!

### 2/ Init KNX device
___
**`void init(HardwareSerial& serial, int progButtonPin, int progLedPin, word manufacturerID, byte deviceID, byte revisionID);`**
* **Description:**  Start the KNX Device. Place this function call in the setup() function of your Arduino sketch
* **Parameters :** serial = serial port that is connected to the KNX TPUART Transceiver, progButtonPin = arduino pin which is connected to the prog button and **is capable of an real interrupt**, progLedPin = arduino pin which is connected to the programming LED, [manufacturerID = your manufacturer ID](konnekting_manufacturers.md), deviceID = ID identifying your device, revision = revision of your firmware (increment if new version is no longer backwards-compatible)

* **Example:** 
```
Konnekting.init(KNX_SERIAL, 
              PROG_BUTTON_PIN, 
              PROG_LED_PIN, 
              MANUFACTURER_ID, 
              DEVICE_ID, 
              REVISION);
```

___
**`void task(void);`**
* **Description:**  KNX device execution task. This function call shall be placed in the "loop()" Arduino function. **WARNING : this function shall be called periodically (400us max period) meaning usage of functions stopping the execution (like delay(), visit http://playground.arduino.cc/Code/AvoidDelay for more info) is FORBIDDEN.**
* **Example:** 
```
Knx.task();
```

___
### 3/ Interact with the communication objects
The API allows you to interact with objects that you have defined : you can read and modify their values, force their value to be updated with the value on the bus. You are also notified each time objects get their value changed following a bus access :
___
**`void knxEvents(byte objectIndex);`**

  _Notify object updates performed via the bus_

* **Description:**  callback function that is called by the KnxDevice library every time a group object is updated by the bus. Define this function in your Arduino sketch.
* **Parameters :** "objectIndex" is the index (in the list) of the object updated by the bus
* **Example:**
```
// Callback function to treat object updates
void knxEvents(byte index) {
  switch (index)
  {
    case 0 : // we arrive here when object index 0 has been updated
      // code to treat index 0 object update
      break;

    case 1 : // we arrive here when object index 1 has been updaed
      // code to treat index 1 object update
      break;

//  ...

    default:
      // code to treat remaining objects updates
      break;
  }
};
```

___
**`byte Knx.read(byte objectIndex);`**

  _Quick method to get the value of a short object_

* **Description:** Get the current value of a short group object. This function is relevant for _short_ objects only, see table below. The returned value will be hazardous in case of use with _long_ objects.
* **Parameters:** "objectIndex" is the index (in the list) of the object to be read.
* **Return:** the current value of the object.
* **Example:** ```Knx.read(0); // return index 0 object value```

| supported KNX DPT formats   |         Remark                                       |
|:---------------------------:|:----------------------------------------------------:|
| KNX_DPT_FORMAT_B1           |                                                      |
| KNX_DPT_FORMAT_B2           |                                                      |
| KNX_DPT_FORMAT_B1U3         | bit fields to be computed by user application        |
| KNX_DPT_FORMAT_A8           |                                                      |
| KNX_DPT_FORMAT_U8           |                                                      |
| KNX_DPT_FORMAT_V8           |                                                      |
| KNX_DPT_FORMAT_B5N3         | bit fields to be computed by user application        |

___
**`e_KnxDeviceStatus Knx.read(byte objectIndex, <any standard C type>& returnedValue);`**

  _Read an usual format com object_

* **Description:** Get the current value of a group object. This function is relevant for objects with usual format, see table below.
* **Parameters:** "objectIndex" is the index (in the list) of the object to be read. "returnedValue" is the read com object value. "returnedValue" can be any standard C type (boolean, uchar, char, uint, int, ulong, long, float, double types).
* **Return:** KNX_DEVICE_OK (0) when everything went well, KNX_DEVICE_NOT_IMPLEMENTED (254) in case of F32 conversion, KNX_DEVICE_ERROR (255) in case of unsupported group object format.
* **Examples:** 
```
byte i; Knx.read(0,i); // read index 0 object (short object)
unsigned int j; Knx.read(1,j); // read index 1 object (U16 format)
int k; Knx.read(2,k); // read index 2 object (V16 format)
unsigned long l; Knx.read(3,l); // read index 3 object (U32 format)
long m; Knx.read(4,m); // read index 4 object (V32 format)
float n; Knx.read(5,n); // read index 5 object (F16/F32 format)
```

| supported KNX DPT formats   |         Remark                                       |
|:---------------------------:|:----------------------------------------------------:|
| KNX_DPT_FORMAT_B1           |                                                      |
| KNX_DPT_FORMAT_B2           |                                                      |
| KNX_DPT_FORMAT_B1U3         | bit fields to be computed by user application        |
| KNX_DPT_FORMAT_A8           |                                                      |
| KNX_DPT_FORMAT_U8           |                                                      |
| KNX_DPT_FORMAT_V8           |                                                      |
| KNX_DPT_FORMAT_B5N3         | bit fields to be computed by user application        |
| KNX_DPT_FORMAT_U16          |                                                      |
| KNX_DPT_FORMAT_V16          |                                                      |
| KNX_DPT_FORMAT_F16          |                                                      |
| KNX_DPT_FORMAT_U32          |                                                      |
| KNX_DPT_FORMAT_V32          |                                                      |
| KNX_DPT_FORMAT_F32          | **!!not yet implemented!!**                          |

___
**`e_KnxDeviceStatus Knx.read(byte objectIndex, byte returnedValue[]);`**

  _Read ANY format com object (advised to advanced users only)_

* **Description:** read the value of a group object. This function supports ALL the DPT formats, the returned value has a rough DPT format.
___
**`e_KnxDeviceStatus Knx.write(byte objectIndex, <any standard C type> value);`**

  _Update any usual format com object_

* **Description:** update the value of a group object. This function is relevant for objects with usual format, see table below.
In case the object has COMMUNICATION and TRANSMIT flags set, then a telegram is emitted on the EIB bus, thus the new value is propagated to the other devices.
* **Parameters:** "objectIndex" is the index (in the list) of the object to be updated. "value" is the new value. value can be any standard C type (boolean, uchar, char, uint, int, ulong, long, float, double types).
* **Return:** KNX_DEVICE_OK (0) when everything went well, KNX_DEVICE_NOT_IMPLEMENTED (254) in case of F32 conversion, KNX_DEVICE_ERROR (255) in case of unsupported group object format.
* **Examples:**
```
byte i=100; Knx.write(0,i); // the object with index 0 gets value 100
int j=-1000; Knx.write(1,j); // the object with index 1 gets value -1000
float k=1234.56; Knx.write(2,k); // the object with index 3 gets value 1234.56
```

| supported KNX DPT formats   |         Remark                                       |
|:---------------------------:|:----------------------------------------------------:|
| KNX_DPT_FORMAT_B1           |                                                      |
| KNX_DPT_FORMAT_B2           |                                                      |
| KNX_DPT_FORMAT_B1U3         | bit fields to be computed by user application        |
| KNX_DPT_FORMAT_A8           |                                                      |
| KNX_DPT_FORMAT_U8           |                                                      |
| KNX_DPT_FORMAT_V8           |                                                      |
| KNX_DPT_FORMAT_B5N3         | bit fields to be computed by user application        |
| KNX_DPT_FORMAT_U16          |                                                      |
| KNX_DPT_FORMAT_V16          |                                                      |
| KNX_DPT_FORMAT_F16          |                                                      |
| KNX_DPT_FORMAT_U32          |                                                      |
| KNX_DPT_FORMAT_V32          |                                                      |
| KNX_DPT_FORMAT_F32          | **!!not yet implemented!!**                          |


___
**`e_KnxDeviceStatus Knx.write(byte objectIndex, byte value[]);`**

  _Update ANY format com object (advised to advanced users only)_

* **Description:** update the value of a group object. This function supports ALL the DPT formats, but a rough DPT format value (previously computed by user application) shall be provided.
___
**`void Knx.update(byte objectIndex);`**

  _Request the local object value to be updated via the bus_

* **Description:** request the (local) group object value to be updated with the value from the bus. Note that this function is _asynchroneous_, the update completion is notified by the knxEvents() callback. This function is relevant only for objects with UPDATE and TRANSMIT flags set.
* **Parameters:** "objectIndex" is the index (in the list) of the object to be updated. 
* **Example:** ```Knx.update(0); // request the update of the object with index 0.```


### 4/ Working with the device
___
**`bool isFactorySetting();`**
* **Description:**  Returns the current device state. Use this before asking for device parameters. If is factory setting, parameters are not yet initialized and do not contain valid values!  

* **Example:** 
```
if (!Konnekting.isFactorySetting()) {
    // now you can be sure that device params contain valid values
}
```

___
**`uint8_t getUINT8Param(byte index);`**  
**`int8_t getINT8Param(byte index);`**  
**`uint16_t getUINT16Param(byte index);`**  
**`int16_t getINT16Param(byte index);`**  
**`uint32_t getUINT32Param(byte index);`**  
**`int32_t getINT32Param(byte index);`**  
* **Description:**  Returns the parameter value which has been programmed via the KONNEKTING Suite. You have to make sure you use the method that matches the parameter type (UINT8, INT8, UNINT16, ...). Otherwise you will get an 0-value.
* **Parameters:** "index" is the ID of your parameter, as it is defined in the [.knxdevice.xml](/konnekting_xml_device_description.md).

* **Example:** 
```
byte myUINT8Param = Konnekting.getUINT8Param(0);
signed int myINT8Param = Konnekting.getINT8Param(1);

int myINT16Param = Konnekting.getINT16Param(2);
unsigned int myUINT16Param = Konnekting.getUINT16Param(3);

long myINT32Param = Konnekting.getINT32Param(4);
unsigned long myUINT32Param = Konnekting.getUINT32Param(5);
```

___
**`bool isReadyForApplication();`**
* **Description:**  Returns the current ready state of the device. You should use this to suspend sketch action (reading sensors etc.) while device is not ready for application (e.g. in programming mode, ...). 
* **Example:** 
```
if (Konnekting.isReadyForApplication()) {
    // do whatever is required for your device, reading sensors, procession I/O, ...
}
```


___
**`int getFreeEepromOffset();`**
* **Description:**  Returns an EEPROM index offset, at which it is save to read/store your device related data. **WARNING: Don't write in the area in front of this index. YOU WILL BREAK YOUR COM-OBJECTS AND DEVICE PARAMETER DATA!!!**
* **Example:** 
```
// writing a 4-byte value to EEPROM
int freeEepromOffset = Konnekting.getFreeEepromOffset();
EEPROM.write(freeEepromOffset, myData[0]);
EEPROM.write(freeEepromOffset+1, myData[1]);
EEPROM.write(freeEepromOffset+2, myData[2]);
EEPROM.write(freeEepromOffset+3, myData[3]);
```
