---
layout: post
title: Configuration
---

For uploading to the WiFire we'll be using MPIDE. MPIDE is based off the Arduino IDE and provides a simple way to program and upload code on a variety of platforms.

MPIDE can be downloaded from the chipKIT website [here](http://chipkit.net/started/install-chipkit-software/).

The modifications to MPIDE are designed for release `20140821`. A list of releases can be found [here](http://chipkit.s3.amazonaws.com/index.html) if a newer update breaks compatibility. 

MPIDE comes with the ability to program the WiFire, but in order to use the Flow libraries we need to add another board configuration.

If you have any problems along the way, either through following the steps in this blog or other issues, you can ask on the [FlowCloud Developer Forums](http://forum.imgtec.com/categories/flow-developers).

## FlowCloud installation

Simply download the [WiFire_Flow.zip]() open it and drag the "hardware" folder into your mpide folder and confirm that you want to merge the folders, replacing all file conflicts.

![MPIDE FlowCloud installation](/flow-on-arduino/images/MPIDE-flowcloud-install.png)

## Programming the WiFire

Now that you have installed FlowCloud into MPIDE you can upload a sample sketch to the WiFire.

Open MPIDE and select "chipKIT WiFire with FlowCloud" as your selected board. If your board is from chipKIT directly and has the stock bootloader then you want to use the 115200bps variant. 
If you have a board with the FlowCloud bootloader (and starter app) or you have flashed the 3Mbps bootloader then choose the 3Mbps option.

![Wifire FlowCloud board select](/flow-on-arduino/images/board-select.png)

Next, plug in your WiFire and select the appropriate port to program it on.
On windows serial ports have the prefix `COM` but on linux they may look more like `/dev/ttyACM<number>` or `/dev/ttyUSB<number>`.
You can see what port your board is on by checking the menu before and after plugging in or by watching the "installing device drivers" dialog on windows.
On Linux you could also check the output of dmesg.

![Wifire serial select](/flow-on-arduino/images/serial-select.png)

Now we need a program. The "Hello World" of the microcontroller world - a blinking light will do perfectly.
MPIDE comes with a variety of example sketches and we are going to use the "Blink" sketch.
Select this from from `File > Examples > 1. Basics > Blink `

![Wifire sketch select](/flow-on-arduino/images/sketch-select.png)

Next, program this onto the WiFire. Make sure `JP2` is connected, making sure that a jumper is connecting the two pins just below the reset button.
Now you can hit "Upload"

![Wifire sketch program](/flow-on-arduino/images/sketch-program.png)

When this is done MPIDE should display the text "Done uploading" in the gray bar below the code.
You are now ready to set up FlowCloud.

## Configuring FlowCloud

Now it's time to configure the WiFire to connect to WiFi and to the cloud.
We'll be using the Android or iOS app for this, but it it possible to configure without these.

For configuring using the console use this tutorial [here](/flow-on-arduino/pages/console-setup).


### Installing the App

First install the relevant App on your smartphone. 

<table>
	<thead>			
		<tr>
			<th>Platform</th>
			<th>App Store Link</th>
		</tr>		
	</thead>		
	<tbody>			
		<tr class="even">
			<td>
				<span class="c-android-logo q-inline"></span>Android
			</td>
			<td>
				<a href="https://play.google.com/store/apps/details?id=com.imgtec.hobbyist" target="_blank">Google Play</a>
			</td>
		</tr>
		<tr class="odd">
			<td>
				<span class="c-ios-logo q-inline"></span>iOS
			</td>
			<td>
				<a href="https://itunes.apple.com/gb/app/makeitflow/id907526535?mt=8" target="_blank">App Store</a>
			</td>
		</tr>
	</tbody>
</table>

### Registering a Device 

Sign up or log in through the app and follow the tutorial for adding a device.
Your WiFire should already be in configuration mode with LED1 flashing.

The password for the WiFi network that you need to connect to should be printed on the box if you got your WiFire with the FlowCloud starter app. If you don't have the password you can find it by opening the Serial Monitor from the top right of MPIDE and setting the baud (changed in the bottom right) to 115200. You should see a line beginning with `Security Key:    ` somewhere in the output. This tells the key you need to connect to the WiFire.

For a more detailed tutorial on how to use the configuration app check out the [developer website](http://flow.imgtec.com/developers/help/wifire/wifire-to-flow). 

Once everything is set up your code of flashing the LED should now be running. You won't be able to use the commands within the app yet, but we'll add those next.

## Extras&#58; Increasing the upload speed

If you are able to flash a new bootloader onto your WiFire, or want to change some driver settings to decrease the time it takes to program your WiFire take a look at the instructions [here](/flow-on-arduino/pages/faster-upload). This step is in no way essential so if you are unsure, don't worry.