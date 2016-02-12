# KONNEKTING XML Device Description

After writing your arduino KONNEKTING Device sketch, you should have a list of communication objects and parameters your device will use or provide. To be able to program the device via the KNX bus, you have to provide a XML file which describes your device. This XML file can then be used by the [KONNEKTING Suite](konnekting_suite.md), which provides a comfortable way of device programming.

Eithe you use a standard text editor (on windows we can recommend [Notepad++](https://notepad-plus-plus.org/)), or you use an XML editor of your choice. In case of an XML Editor that can handle XML Schema Definition files (.xsd), you can use this one:

https://github.com/KONNEKTING/KonnektingXmlSchema/blob/master/src/main/xsd/KonnektingDeviceV0.xsd

# Preparation

If you haven't registered a manufacturer-id yet, NOW would be the best time for it. **TODO: ADD LINK HERE**

# XML Format explained

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<KonnektingDevice xmlns="http://konnekting.de/xml/KonnektingDevice/v0">
    <Device ManufacturerId="12345" DeviceId="123" Revision="123">
        <ManufacturerName>You Manufacturer Name</ManufacturerName>
        <DeviceName>Your Device Name</DeviceName>
        <Parameters>
            <Group name="A Parameter Group">
                <Parameter Id="0">
                    <Description>A Parameter</Description>
                    <Value Type="uint8" Default="01" Options="" Min="00" Max="0A"/>
                </Parameter>
                <Parameter Id="1">
                    <Description>Another Parameter</Description>
                    <Value Type="int16" Default="0001" Options=""/>
                </Parameter>
                <Parameter Id="2">
                    <Description>Yet Another Parameter</Description>
                    <Value Type="uint32" Default="00000000" Options="00000000=10ms|00000001=30ms|00000002=60ms|00000004=120ms|FFFFFFFF=MAX"/>
                </Parameter>
            </Group>
            <Group name="Another Parameter Group">
                <Parameter Id="4">
                    <Description>Verhalten nach Busspannungsausfall</Description>
                    <Value Type="uint8" Default="02" Options="00=Aus|01=An|02=letzter Wert|04=Helligkeitswert"/>
                </Parameter>
        </Parameters>
        <CommObjects>
            <CommObject Id="0">
                <Name>My First Com Object</Name>
                <Function>Test-Funtion #1</Function>
                <DataPointType>1</DataPointType>
            </CommObject>
            <CommObject Id="1">
                <Name>My Second Com Object</Name>
                <Function>Test-Funtion #2</Function>
                <DataPointType>2</DataPointType>
            </CommObject>
        </CommObjects>
    </Device>
</KonnektingDevice>
```
