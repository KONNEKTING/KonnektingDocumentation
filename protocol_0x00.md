## Message Types for Protocol-Version 0x00

There are several message types for all kind of purposes. As we have one byte for the Message-Type-ID, we have up to 256 possible messages. That should be enough for our purposes.

### Acknowledge

**Message Name:** Acknowledge  
**MsgID:** 0 dec / 0x00 hex  
**Description:** Acknowledge a previous received WRITE message  
**Requires Programming Mode:** no 

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
**Requires Programming Mode:** no

<table>
    <tr><th>Byte no#</th><th>Description</th></tr>
    <tr><td>2</td><td>Manufacturer-ID HI</td></tr>
    <tr><td>3</td><td>Manufacturer-ID LO</td></tr>
    <tr><td>4</td><td>Device-ID</td></tr>
    <tr><td>5</td><td>Revision-ID</td></tr>
    <tr><td>6</td><td>Device Flags<br><b>!TO BE DEFINED!</b></td></tr>
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

----


!!! It's required that no other device is using this address. Otherwise it may conflict with KONNEKTING Devices!
