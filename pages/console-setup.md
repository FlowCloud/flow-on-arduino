---
layout: page
title: Command Line Setup
---

This tutorial will cover setting up the WiFire without using the smartphone app. This may be required if there are issues in using the app or you may just prefer to do it this way.

## Serial Connection

While MPIDE provides a Serial Monitor this lacks easy support for entering commands that need interactive input.
Instead we can use the much more powerful PuTTY. You can download PuTTY [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). Pick `putty.exe` from the list of downloads. PuTTY can run straight from being downloaded and does not need to be installed.

If you are using an operating system other than Windows then you will need to find an alternate serial console.

Launch PuTTY or your chosen serial console and connect to the serial port you used for programming. 
Make sure that "Serial" is selected and that the Speed or Baud is set to 115200 and open the connection.
You should see the following:

```
System booting - Initialising appcore



 --- Flow PIC32MZ WiFire Starter App v1.1.0 ---
              [Configuration Mode]



Flow PIC32MZ WiFire Starter App v1.1.0 startup - Configuration mode
TCP/IP Stack Initialization Started
TCP/IP Stack Initialization Ended - success

==========================
*** WiFi EZConfig Demo ***
==========================
Device:          MRF24WG (0x310C)
Domain:          FCC
MAC:             00 1E C0 17 5E 67
SSID:            WiFire_175E67
Network Type:    SoftAP
Scan Type:       Passive Scan
Channel List:    6
Beacon Timeout:  40
Retry Count:     255 (Retry Forever)
Security:        WEP40, Open Key
Security Key:    aabbccddee
Tx Mode:         802.11bg mixed
Power Save:      disabled
IP Config:       dynamic

Start WiFi Connect . . .
MRF24W IP Address: 0.0.0.0
MRF24W Event: Connection Successful
  Connected BSSID:  00:00:00:00:00:00
  Channel:          0

```

In order to connect to FlowCloud you'll need to register a developer account and get your registration key.
Go to the [FlowCloud developers page](http://flow.imgtec.com/developers/) and click the sign-up button at the top of the page.

Once you have an account and are logged in go to "My Devices" by either selecting it from the menu, selecting "View My Devices" on your account dashboard or following [this](http://flow.imgtec.com/developers/devices) link.

Select "Registration" and choose WiFire from the "Device Type" menu.

![Device registration](/flow-on-arduino/images/device-registration.png)

Select "Get New Code" under registration method B. We'll use this code for configuring the WiFire.

Now we have a registration code we can continue the setup.
Enter into the serial console
`set devreg_key` 
an press enter.
Press `y` at the prompt then enter your registration key and press enter. 
PuTTY pastes text when you right click, so you can copy and paste this key.

Next we need to select a WiFi network to connect to.

Enter the command `set network_config` and press enter.

Select `y` to the prompt for setting a SSID and enter the name for your WiFi network. Make sure to use the correct capitalisation.

If the encryption type shown is correct there is no need to change it so you can press `n` for not changing otherwise press `y` and change it.

```
>set network_config
Current WiFi network SSID: HomeWifi
Do you want to set a new WiFi network SSID: (y/n) n
Current network encryption type: WPA2
Do you want to set a new WiFi network encryption type: (y/n)
```

In my case I use WPA2 and that is what the current setting is and what my WiFi network uses so I press `n`.

Next a password needs to be set. Repeat the process for entering the network name. Select `y`, enter the password and press enter.

```
>set network_config
Current WiFi network SSID: IMGWE-TEST
Do you want to set a new WiFi network SSID: (y/n) n
Current network encryption type: WPA2
Do you want to set a new WiFi network encryption type: (y/n) n
Current WiFi network password: not set
Do you want to set a new WiFi network password: (y/n) y
Enter new WiFi network password: hunter2
Setting updated successfully.
Current addressing scheme: dhcp
Do you want to set a new addressing scheme: (y/n)
```

Next, unless you know that you do not use DHCP select `n` for changing away the addressing scheme.

Finally select `y` to reboot into interactive mode on next reboot.

If everything was entered correctly type `reboot` and press enter, otherwise you can issue the `set network_config` command and try again. 

If something fails in the boot process then you can issue the same commands then reboot to fix the problem. Otherwise you should see the board connect to FlowCloud print

```
----------------------------------------------
Flow Android App booting completed. Running..
```

The code you programmed inside MPIDE should now be running.