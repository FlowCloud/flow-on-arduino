---
layout: post
title: Introduction
---

In this blog I will detail my progress of using [FlowCloud](http://flow.imgtec.com/developers/) within the Arduino environment. FlowCloud is a platform designed to allow developers and hobbyists to develop for the Internet of Things.
I'll be using the [WiFire](http://www.digilentinc.com/wifire/) development board that can be seen below.
![](/flow-on-arduino/images/chipKIT-WiFIRE-obl-600.png)

Before we get started let's look at the key details about FlowCloud and the WiFire.

## FlowCloud

Using FlowCloud we will gain access to a large suite of functionality designed to connect devices to the Internet of Things.
Using FlowCloud on Arduino you can easily connect your Arduino projects to each other. We can do anything from controlling your home while away to building a network of sensors to track anything you like.

With the FlowCloud datastore we can store all the data in the Cloud and access it from any connected device or from your computer and with FlowMessaging devices and their users can easily interact with each other. 

FlowCloud removes the headache of having to deal with scalability, security and connectivity and takes care of all of this for you! 


Check out the Flow developer [Getting Started](http://flow.imgtec.com/developers/getting-started) page for more ideas of what you can do.

![](/flow-on-arduino/images/flow_diagram_home_small.png)

## WiFire

The WiFire offers a powerful, feature-packed development board, complete with onboard WiFi and is compatible with Arduino shields.

#### Spec summary
<ul>
	<li>
		Processor:
		<ul>
			<li>
				Microchip® PIC32MZ2048ECG
				<ul>
					<li>200 MHz 32-bit MIPS</li>
					<li>2MB Flash</li>
					<li>512K SRAM</li>
				</ul>
			</li>
		</ul>
	</li>
	<li>Microchip MRF24WG0MA WiFi module</li>
	<li>Micro SD card connector</li>
	<li>USB 2.0 Full-Speed / Hi-Speed OTG</li>
	<li>6x UART</li>
	<li>6x 50MHz SPI</li>
	<li>5x I2C/2-wire bus</li>
	<li>43 available I/O pins
		<ul>
			<li>10 bit ADC</li>
			<li>
				on-board user interfaces: 
				<ul>
					<li>4 LED</li>
					<li>2 Buttons</li>
					<li>1 Potentiometer</li>
				</ul>
			</li>
		</ul>
	</li>
</ul>


<br><br>
Read on for how to get set up for programming the WiFire...