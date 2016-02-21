## Message Types for Protocol-Version 0x00

This is the first protocol version.

## Message types
- [0x00 Acknowledge](#acknowledge)
- [0x01 ReadDeviceInfo](#readdeviceinfo)
- [0x02 AnswerDeviceInfo](#answerdeviceinfo)
- [0x09 Restart](#restart)
- ...
- [0x0A WriteProgrammingMode](#writeprogrammingmode)
- [0x0B ReadProgrammingMode](#readprogrammingmode)
- [0x0C AnswerProgrammingMode](#answerprogrammingmode)
- ...
- [0x14 WriteIndividualAddress](#writeindividualaddress)
- [0x15 ReadIndividualAddress](#readindividualaddress)
- [0x16 AnswerIndividualAddress](#answerindividualaddress)
- ...
- [0x1E WriteParameter](#writeparameter)
- [0x1F ReadParameter](#readparameter)
- [0x20 AnswerParameter](#answerparameter)
- ...
- [0x28 WriteCommObject](#writecommobject)
- [0x29 ReadCommObject](#readcommobject)
- [0x2A AnswerCommObject](#answercommobject)
- ...

  
  
### Acknowledge

**Message Name:** Acknowledge  
**MsgID:** 0 dec / 0x00 hex  
**Description:** Acknowledge a previous received WRITE message  
**Requires Programming Mode:** n/a 

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>
         <b>Acknowledge type:</b><br>
         0x00: ACK<br>
         0xFF: NACK
       </td></tr>
    <tr><td>3</td><td>
         <b>Error-Code (only NACK):</b><br>
         0x00: no error / no error code available<br>
         0x01: invalid index<br>
         0xFE: not implemented<br>
         0xFF: general error<br>
       </td></tr>
    <tr><td>4</td><td>
         <b>Affected index (only NACK):</b><br>
         0x00..0xFE: errornous index<br>
         0xFF: no relevant index<br>
       </td></tr>
    <tr><td>5</td><td align="center" rowspan="9">0x00, unused</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>

### ReadDeviceInfo

**Message Name:** ReadDeviceInfo  
**MsgID:** 1 dec / 0x01 hex  
**Description:** Initiates reading general device information    
**Requires Programming Mode:** no

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>IndividualAddress HI</td></tr>
    <tr><td>3</td><td>IndividualAddress LO</td></tr>
    <tr><td>4</td><td align="center" rowspan="11">0x00, unused</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>


### AnswerDeviceInfo

**Message Name:** AnswerDeviceInfo  
**MsgID:** 2 dec / 0x02 hex  
**Description:** Answers a ReadDeviceInfo message  
**Requires Programming Mode:** n/a

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>Manufacturer-ID HI</td></tr>
    <tr><td>3</td><td>Manufacturer-ID LO</td></tr>
    <tr><td>4</td><td>Device-ID</td></tr>
    <tr><td>5</td><td>Revision-ID</td></tr>
    <tr><td>6</td><td><b>Device Flags</b><br>
Flag masks: <br/>0x80: Factory-Flag: 1 = factory settings, 0 = EEPROM settings<br/></td></tr>
    <tr><td>7</td><td>IndividualAddress HI</td></tr>
    <tr><td>8</td><td>IndividualAddress LO</td></tr>
    <tr><td>9</td><td align="center" rowspan="5">0x00, unused</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>


### Restart

**Message Name:** Restart  
**MsgID:** 9 dec / 0x09 hex  
**Description:** Restart device (device reboot, no memory reset)  
**Requires Programming Mode:** no

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>IndividualAddress HI</td></tr>
    <tr><td>3</td><td>IndividualAddress LO</td></tr>
    <tr><td>4</td><td align="center" rowspan="10">0x00, unused</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>

### WriteProgrammingMode

**Message Name:** WriteProgrammingMode  
**MsgID:** 10 dec / 0x0A hex  
**Description:** Sets/Unsets Programming-Mode for device with given individual address. Device will respond with a "Acknowledge" message.  
**Requires Programming Mode:** no

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>IndividualAddress HI</td></tr>
    <tr><td>3</td><td>IndividualAddress LO</td></tr>
    <tr><td>4</td><td><b>Mode:</b><br>
							0x00 = OFF<br>
							0x01 = ON</td></tr>
    <tr><td>5</td><td align="center" rowspan="9">0x00, unused</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>


### ReadProgrammingMode

**Message Name:** ReadProgrammingMode  
**MsgID:** 11 dec / 0x10 hex  
**Description:** Reads current programming mode of all listening devices. If more than one device is in programming-mode, you will get more then one AnswerProgrammingMode messages.    
**Requires Programming Mode:** no

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td align="center" rowspan="12">0x00, unused</td></tr>
    <tr><td>3</td></tr>
    <tr><td>4</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>

### AnswerProgrammingMode

**Message Name:** AnswerProgrammingMode  
**MsgID:** 12 dec / 0x11 hex  
**Description:** Answers a ReadDeviceInfo message with the device's individual address.  
**Requires Programming Mode:** n/a


<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>IndividualAddress HI</td></tr>
    <tr><td>3</td><td>IndividualAddress LO</td></tr>
    <tr><td>4</td><td align="center" rowspan="10">0x00, unused</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>

### WriteIndividualAddress

**Message Name:** WriteProgrammingMode  
**MsgID:** 20 dec / 0x14 hex  
**Description:** Writes the individual address the device should use. Device will respond with a "Acknowledge" message.			  
**Requires Programming Mode:** yes

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>IndividualAddress HI</td></tr>
    <tr><td>3</td><td>IndividualAddress LO</td></tr>
    <tr><td>4</td><td align="center" rowspan="10">0x00, unused</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>


### ReadIndividualAddress

**Message Name:** ReadIndividualAddress  
**MsgID:** 21 dec / 0x15 hex  
**Description:** Each device in programming mode will respond with its individual address			    
**Requires Programming Mode:** yes

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td align="center" rowspan="12">0x00, unused</td></tr>
    <tr><td>3</td></tr>
    <tr><td>4</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>

### AnswerIndividualAddress

**Message Name:** AnswerIndividualAddress  
**MsgID:** 22 dec / 0x16 hex  
**Description:** Answers a ReadDeviceInfo message with the device's individual address.  
**Requires Programming Mode:** n/a


<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>IndividualAddress HI</td></tr>
    <tr><td>3</td><td>IndividualAddress LO</td></tr>
    <tr><td>4</td><td align="center" rowspan="10">0x00, unused</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>


### WriteParameter

**Message Name:** WriteParameter  
**MsgID:** 30 dec / 0x1E hex  
**Description:** Writes a value for a specific parameter. Device will respond with a "Acknowledge" message.  			  
**Requires Programming Mode:** yes

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>Parameter ID</td></tr>
    <tr><td>3</td><td align="center" rowspan="11">0x00, unused</td></tr>
    <tr><td>4</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>


### ReadParameter

**Message Name:** ReadParameter  
**MsgID:** 31 dec / 0x1F hex  
**Description:** Reads a value for a specific parameter  
**Requires Programming Mode:** yes

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>Parameter ID</td></tr>
    <tr><td>3</td><td align="center" rowspan="11">0x00, unused</td></tr>
    <tr><td>4</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>

### AnswerParameter

**Message Name:** AnswerParameter  
**MsgID:** 32 dec / 0x20 hex  
**Description:** Answers a ReadDeviceInfo message with the device's individual address.  
**Requires Programming Mode:** n/a


<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>Parameter ID</td></tr>
    <tr><td>3</td><td align="center" rowspan="11">parameter value<br/>up to 11 bytes, depends on the parameter</td></tr>
    <tr><td>4</td></tr>
    <tr><td>5</td></tr>
    <tr><td>6</td></tr>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>

### WriteCommObject

**Message Name:** WriteCommObject  
**MsgID:** 40 dec / 0x28 hex  
**Description:** Writes up to 3 GAs for a specific CommObjects. Device will respond with a "Acknowledge" message.  
**Requires Programming Mode:** yes

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>Number of CommObjects to write,<br/>range [1..3]</td></tr>    
    <tr><td>3</td><td>#1 CommObject ID</td></tr>
    <tr><td>4</td><td>#1 GA HI</td></tr>
    <tr><td>5</td><td>#1 GA LO</td></tr>
    <tr><td>6</td><td>#2 CommObject ID</td></tr>
    <tr><td>7</td><td>#2 GA HI</td></tr>
    <tr><td>8</td><td>#2 GA LO</td></tr>
    <tr><td>9</td><td>#3 CommObject ID</td></tr>
    <tr><td>10</td><td>#3 GA HI</td></tr>
    <tr><td>11</td><td>#3 GA LO</td></tr>
    <tr><td>12</td><td align="center" rowspan="2">0x00, unused</td></tr>
    <tr><td>13</td></tr>
</table>


### ReadCommObject

**Message Name:** ReadCommObject  
**MsgID:** 41 dec / 0x29 hex  
**Description:** Reads up to 3 GAs for a specific CommObjects.   
**Requires Programming Mode:** no

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>Number of CommObjects to read,<br/>range [1..3]</td></tr>
    <tr><td>3</td><td>ComObjectID #1</td></tr>
    <tr><td>4</td><td>ComObjectID #2</td></tr>
    <tr><td>5</td><td>ComObjectID #3</td></tr>
    <tr><td>6</td><td align="center" rowspan="8">0x00, unused</td>
    <tr><td>7</td></tr>
    <tr><td>8</td></tr>
    <tr><td>9</td></tr>
    <tr><td>10</td></tr>
    <tr><td>11</td></tr>
    <tr><td>12</td></tr>
    <tr><td>13</td></tr>
</table>

### AnswerCommObject

**Message Name:** AnswerCommObject  
**MsgID:** 42 dec / 0x2A hex  
**Description:** Answers read request for up to 3 specific CommObjects. i.e. 
number of tupels = 2, 
then byte 3 to 8 contains two tupels. Rest is filled with 0x00."  
**Requires Programming Mode:** n/a


<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>Number of tupels,<br/> range [1..3]</td></tr>
    <tr><td>3</td><td>#1 CommObject ID</td></tr>
    <tr><td>4</td><td>#1 GA HI</td></tr>
    <tr><td>5</td><td>#1 GA LO</td></tr>
    <tr><td>6</td><td>#2 CommObject ID</td></tr>
    <tr><td>7</td><td>#2 GA HI</td></tr>
    <tr><td>8</td><td>#2 GA LO</td></tr>
    <tr><td>9</td><td>#3 CommObject ID</td></tr>
    <tr><td>10</td><td>#3 GA HI</td></tr>
    <tr><td>11</td><td>#3 GA LO</td></tr>
    <tr><td>12</td><td align="center" rowspan="2">0x00, unused</td></tr>
    <tr><td>13</td></tr>
</table>
