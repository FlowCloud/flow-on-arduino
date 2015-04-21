---
layout: post
title: Geofence Application&#58; The Plan
---

With the basics out of the way, it's now time to work on a "real" project.

For this project we will create an application for logging and generating alerts using a GPS location read by the WiFire.

To read a GPS location we will be using a Bluetooth GPS data logger. We will then use a Bluetooth to RS232 module to receive the Bluetooth data, and then a RS232 Arduino shield on the WiFire.

While GPS NMEA -> Bluetooth -> RS232 -> Serial input -> FlowCloud (through WiFi connection) is a little odd it demonstrates how the WiFire can interact with other systems. RS232 uses UART, a common form of serial communication and there are many projects where one may wish to interface the WiFire with some other equipment. 

GPS data also provides a good example of data that can both be logged and have events generated from it (i.e. a geofence). It also allows us to leverage existing code for Arduino to interact with a GPS. 

<hr>

I'll be using git to manage my development and you can access the [repository here](https://github.com/FlowCloud/geofence). 

## List of materials  

### WiFire
<img src="/flow-on-arduino/images/wifire.png" width="300"></img>

### RS232 Shield
<img src="/flow-on-arduino/images/rs232.jpg" width="300"></img>

Available from [sparkfun](https://www.sparkfun.com/products/13029).

### Bluetooth to RS232 module 
<img src="/flow-on-arduino/images/bluetooth.jpg" width="300"></img>

### GPS Bluetooth data logger
<img src="/flow-on-arduino/images/gps.jpg" width="200"></img>

Any will do, as long as it provides serial NMEA data over Bluetooth.

### DB9 Crossover cable
We'll also need to make a RS232 crossover cable to allow the RS232 shield to receive data from the Bluetooth module.
This can easily be made with two DB9 male connectors and some cable.
<img src="/flow-on-arduino/images/db9.jpg" width="200"></img>

