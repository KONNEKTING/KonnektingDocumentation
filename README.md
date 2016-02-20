# KONNEKTING Documentation

# Introduction

In January 2014, there was a discussion about developing a do-it-yourself KNX device with help of Arduino and an Siemens buscoppler on the german [KNX User Forum](http://knx-user-forum.de/forum/%C3%B6ffentlicher-bereich/knx-eib-forum/diy-do-it-yourself/33016-arduino-am-knx).

People where happy and started to develop own stuff. Months later, the idea to have a "over the bus programmable" DIY device rised. Early attempts in extening the [DKA TpUart](https://bitbucket.org/dka/arduino-tpuart) library with "property read/write, memory read/write) failed with strange timing issues. 
Again a fews weeks later, a few guys put their heads together to create a new attempt. And **this** is the documentation about this attempt.

It's a complete open source project with an library for Arduino boards that allows KNX access and a smart software tool that allows programming those DIY devices via KNX bus.

But first of all, this child needs a name:

***KONNEKTING***

The name is a wordplay with "connect”, “connecting” and “Ing”, the german abbreviation for "engineer". If you combine them, you will get “KONNEKTING”.


KONNEKTING is the generic term for the following two high-level projects:

* KONNEKTING Device Library, the library for your Arduino
* KONNEKTING Suite, the software to parametrize your Arduino KNX device

There are some more sub-project, but they are more or less low-level and not so important at this stage.

# KONNEKTING Device Library

The library for your Arduino is based on the development of [Franck Marini](https://github.com/franckmarini/KnxDevice), with an extension that allows programming via the KNX bus.
We call such devices "KONNEKTING device" or "kdevice".

# KONNEKTING Suite

The software for programming/parametrizing is a platform independant, graphical desktop application. It runs on Windows, Linux and Mac. To keep it short, we just name it "Suite".

An XML file defines what your kdevice can do (and what not). So each devices type needs such a "device definition" with the file suffix “.kdevice.xml”. The XML contains a name/ID for the device, which communication-objects it has and of course what parameter you can set/configure. It's kind of similar to the "ETS product database" (.knxprod/.vd4) file you should already know.

With help of this XML file you can add new devices, give them an individual address, associate group addresses with the communication objects, setup parameters and finally push the complete configuration via KNX bus to the device with just one click (and one initial button press on the device).


## Contents

- [KONNEKTING Device Library](konnekting_device_library.md)
- [KONNEKTING XML based device description](konnekting_xml_device_description.md)
- [KONNEKTING Suite](konnekting_suite.md)
- [Telegram-based programming protocol](protocol_general.md)
- [Programming Workflow](programming_workflow.md)
