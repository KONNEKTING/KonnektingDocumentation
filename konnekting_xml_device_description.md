# KONNEKTING XML Device Description

After writing your arduino KONNEKTING Device sketch, you should have a list of communication objects and parameters your device will use or provide. To be able to program the device via the KNX bus, you have to provide a XML file which describes your device. This XML file can then be used by the [KONNEKTING Suite](konnekting_suite.md), which provides a comfortable way of device programming.

Eithe you use a standard text editor (on windows we can recommend [Notepad++](https://notepad-plus-plus.org/)), or you use an XML editor of your choice. In case of an XML Editor that can handle XML Schema Definition files (.xsd), you can use this one:

https://github.com/KONNEKTING/KonnektingXmlSchema/blob/master/src/main/xsd/KonnektingDeviceV0.xsd

# Preparation

If you haven't registered a manufacturer-id yet, NOW would be the best time for it. [REGISTER FOR FREE](konnekting_manufacturers.md)

# XML Format explained



```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<KonnektingDevice xmlns="http://konnekting.de/xml/KonnektingDevice/v0">
    
    <!-- 
        Herer you have to provide your IDs and device revision. 
        This MUST match the definition in your Arduino Sketch.
        Otherwise programming fails.
        
        The IDs need to be provided as unsigned integer values.
        
        Manufacturer ID = Your individual vendor ID (uint16)
        Device ID = An ID for your device (uint8) 
        Revision = typically "1" (uint8)
                    You might use 
                        0 for developing-phase, 
                        1 for first release and 
                        2...255 of every change which breaks compatibility.
    -->
    <Device ManufacturerId="12345" DeviceId="123" Revision="123">

        <!-- 
            Providing a manufacturer name and device name is optional.
            But without, your device will not be shown with a vendor/name in the KONNEKTING SUite
        -->
        <ManufacturerName>You Manufacturer Name</ManufacturerName>
        <DeviceName>Your Device Name</DeviceName>
        
        <!--
            If your device needs any parameters which the user should be able to configure, 
            you have to put them here.
        -->
        <Parameters>
        
            <!--
                Parameters are grouped. It's not allowed to have a parameter without a group.
                
                Each group needs an ID and readable name. The first group has to have Id="0". 
                The ID must be provided as a readable integer and you must not have Id-gaps.
                The name is display in the KONNEKTING Suite.

                You can have as much groups as you like. Okay, actually the limit is 256. 
                But that should be enough.
            -->
            <ParameterGroup Name="A Parameter Group" Id="0">
            
                <!--
                    Each parameter needs an ID. The first parameter has to have Id="0". 
                    The ID must be provided as a readable integer.
                    
                    You can distribute your parameters in any order over the groups. 
                    Important thing is: 
                        - If you have parameters, there must a parameter with Id="0", 
                        - There are no Id-gaps
                        
                    If you have an obsolete parameter: 
                        - change the name to "unused" or "deprecated" or whatever,
                        - set type to uint8,
                        - Default to 00,
                        - Min to 00 and
                        - Max to 00
                        
                    This will just waste one byte of your EEPROM memory in your sketch. 
                    
                    IdName is optional and can be used together with the KONNEKTING Code-Creator to
                    auto-create Arduino-code. Ensure that the IdName attribute has only alphabetic chars and no whitespaces, special-chars etc.
                -->
                <Parameter Id="0" IdName="aParameter">
                    
                    <!-- If course a parameter needs a name, which you can define here -->
                    <Description>A Parameter</Description>
                    
                    <!--
                        And beside a name, you have to provide the at least:
                        - Type: 
                            The variable type of your number parameter
                            
                            uint8: unsigned 8-bit integer: 0..255
                            int8: signed 8-bit integer: -128..127
                            
                            uint16: unsigned 16-bit integer: 0..65535
                            int16: signed 16-bit integer: -32768..32767
                            
                            uint32: unsigned 32-bit integer: 0..4294967295
                            int32: signed 32-bit integer: -2147483648..2147483647
                            
                            And there is a type for raw data, called "raw". This is f.i. useful 
                            if you have a parameter for IR hexcodes or 1-wire serialnumbers etc.
                            The Suite will let you enter the data as HEX.
                            
                            raw1: 1 byte raw data
                            raw2: 2 byte raw data
                            .
                            .
                            raw11: 11 byte raw data
                            
                            string11: max. 11 byte ISO-8859-1 encoded string/text. Unused, tailing characters are filled with 0x00
                            
                            This attribute is MANDATORY and there's - except for DEFAULT - no other attribute possible!
                            
                        - Default
                            the hexadecimal representation of the default-value, 
                            according to the given type, incl. leading/tailing zeros, f.i.
                            a 16-bit type needs four hex-characters: 00FF
                            a 32-bit type needs eight hex-characters: 000000FF
                            a 11-byte-string: 666F6F2062617200000000 ("foo bar")
                            This attribute is MANDATORY.
                            
                        Optional:
                        - Options
                            If you want to present a prset of options for a parameter, 
                            you can use the "Options" attribute. The value for this 
                            attribute is a set of "pipe" separated value-key-pairs:
                            
                            Options="value=key_1|value=key_2|value=key_n"
                            
                            The "key" is what the user will see in the dropdown box in 
                            KONNEKTING Suite, als "value" is what interally will be chosen 
                            as the value for the parameter if selected.
                            
                            Example: You want to preset a preset of "startup delay values" to the user:
                            10ms, 30ms, 60ms, 120ms, no delay
                            
                            Then the options could look like this:
                            
                            Options="00=10ms|01=30ms|02=60ms|04=120ms|FF=no delay"
                            
                            Your arduino sketch needs to know how to interpret the values and that 
                            the 02hex value actually means 60ms and FFhex means no delay.
                            
                        - Min/Max
                            Can only be used when no Options attribute is set and with a number type. This will limit the 
                            value the user is allowed to enter between Min and Max. The provided
                            value needs to be hex again with leading zeros according to the type.
                    -->
                    <Value Type="uint8" Default="01" Options="" Min="00" Max="0A"/>
                    
                </Parameter>
                
                <Parameter Id="1" IdName="anotherParameter">
                    <Description>Another Parameter</Description>
                    <Value Type="int16" Default="0001" Options=""/>
                </Parameter>
                
                <Parameter Id="2" IdName="yetAnotherParameter">
                    <Description>Yet Another Parameter</Description>
                    <Value Type="uint8" Default="00" Options="00=10ms|01=30ms|02=60ms|04=120ms|FF=no delay"/>
                </Parameter>
                
                <Parameter Id="3" IdName="activate1">
                    <Description>Activate 'Another Parameter'</Description>
                    <Value Type="uint8" Default="01" Options="00=disabled|01=enabled"/>
                </Parameter>
                
                <Parameter Id="4" IdName="activate2">
                    <Description>Activate 'Another Parameter Group'</Description>
                    <Value Type="uint8" Default="01" Options="00=disabled|01=enabled"/>
                </Parameter>
                
                <Parameter Id="5" IdName="activate3">
                    <Description>Activate 'My Second Com Object'</Description>
                    <Value Type="uint8" Default="01" Options="00=disabled|01=enabled"/>
                </Parameter>                
            </ParameterGroup>
            
            <ParameterGroup Id="1" Name="Another Parameter Group">
                <Parameter Id="6">
                    <Description>Another parameter in another parametergroup</Description>
                    <Value Type="uint8" Default="02" Options="00=Aus|01=An|02=letzter Wert|04=Helligkeitswert"/>
                </Parameter>
            </Group>
            
        </ParameterGroup>
        
        <CommObjects>
            <!-- 
                A normal KNX device should have at least one communication object.
                As with the parameters, IDs must start at 0 and there must be no ID gaps. 
                The maximum number of comm objects is 256. The Id must be provided as a readable integer.
            -->
            <CommObject Id="0">
                <!-- You should provide a reasinaly name -->
                <Name>My First Com Object</Name>
                <!-- and a description of the functiono of this comm object-->
                <Function>Test-Function #1</Function>
                <!-- 
                    Here you have to provide the Datapoint Type for this CommObject
                    Format:
                        x.yyy
                    where x = main type number, without leading zeros
                    and yyy = sub type number, with leading zeros to have 3 characters
                -->
                <DataPointType>1.001</DataPointType>
                
                <!-- 
                    ComObj communication flags. 
                    
                    The "Flags" is a single byte (integer value), 
                    that indicates the set communication flags of the ComObj by setting/unsetting single bits of that byte.
                    
                    Flags are:
                        C   Communication
                        R   Read
                        W   Write
                        T   Transmit
                        U   Update
                        I   Init
                        
                    (for more details about the flags, read: http://www.knx.org/fileadmin/template/documents/downloads_support_menu/KNX_tutor_seminar_page/Advanced_documentation/02_Flags_E1008a.pdf)
                    
                     B7  B6  B5  B4  B3  B2  B1  B0 (Bit number)
                    128  64  32  16   8   4   2   1 (integer value)
                     xx  xx   C   R   W   T   U   I (Flag) 
                    
                    Bit B6 and B7 are unused.
                    
                    Common flag-combinations:
                        * "Sensor Profile" -> C+R+T -> 32+16+8 = 52 (integer value)
                        * "Logical Input Profile" -> C+W+U -> 32+8+2 = 42 (integer value)
                -->
                <Flags>52</Flags>
            </CommObject>
            
            <CommObject Id="1">
                <Name>My Second Com Object</Name>
                <Function>Test-Function #2</Function>
                <DataPointType>2</DataPointType>
                <Flags>42</Flags>
            </CommObject>
        </CommObjects>
        
        <!--
            If you want to hide CommObjects, Parametergroups or Parameters depending on parameter values,
            you can define dependencies here.
            
            ParameterDependency:        T
                The visibility of a parameter depends on the value of another parameter
                * ParamId references the affected Parameter
                
            ParameterGroupDependency:   
                The visibility of a parameter group depends on the value of another parameter
                * ParamGroupId references the affected ParameterGroup
                
            CommObjectDependency:       
                The visibility of a CommObject depends on the value of a parameter
                * CommObjId references the affected CommObject
                
            All three depedency types have three common attributes:
            
                * Test: one of
                    - eq = equals
                    - gt = greater than
                    - lt = less than
                    - ge = greater or equals than
                    - le = less or equals than
                * TestParamId: the parameter which's value is been tested for setting visibility
                * TestValue: the value the parameter is tested for
            
            !!! There's one limitation: The parameters that is used as a dependency (=referenced by TestParamId) needs
            the parameter type "uint8". Other types are not supported.
        -->
        <Dependencies>
            <ParameterDependency ParamId="1" Test="eq" TestParamId="3" TestValue="01"/>
	        <ParameterGroupDependency ParamGroupId="1" Test="eq" TestParamId="4" TestValue="01"/>
            <CommObjectDependency CommObjId="1" Test="eq" TestParamId="5" TestValue="01"/>
        </Dependencies>
    </Device>
</KonnektingDevice>
```
