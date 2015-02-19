---
layout: page
title: Increasing the Upload Speed
---

This page is intended for a those curious about some of the internals of how the WiFire (and other boards) works and how this can help achieve shorter programming times.
It somewhat more technical than other parts of the tutorial series.

There a two major ways you can increase the upload speed. By following these steps the upload time can be reduced to only 20s.

## Flashing a new Bootloader

This method requires a device that can act as an in-circuit serial programmer (ICSP). A variety of devices are available to do this such as the [chipKIT PGM](https://www.digilentinc.com/Products/Detail.cfm?Prod=chipKIT%20PGM), the [Microchip PICkit](http://www.microchip.com/Developmenttools/ProductDetails.aspx?PartNO=PG164130&utm_source=&utm_medium=MicroSolutions&utm_term=&utm_content=DevTools&utm_campaign=PICkit+3) along with many others.You may even be able to program another microcontroller [as seen here](http://www.gammon.com.au/bootloader), although this would require implementing the ICSP protocol used by the PIC chips.

### Background
chipKIT provides the WiFire with a bootloader implementing the STK500v2 protocol. This allows the tool avrdude, originally designed for uploading to AVR chips, to program the WiFire without needing a dedicated device to program them. This can be useful as it means there is no need to purchase or use an additional piece of hardware. Instead the STK500 bootloader can receive the program over the serial connection and re-program the chip itself.

### What we can change
The default bootloader uses a set speed of 115200 bits per second - just over 110 kbits/s.
This is slow. The WiFire can handle receiving data at 3000000 bits/s or 3 Mbits/s.
By modifying the bootloader source provided [here](https://github.com/chipKIT32/PIC32-avrdude-bootloader) by chipKIT we can bump the expected baud rate of the WiFire bootloader all the way up to 3 Mbps.

A version with this change can be found [here](/downloads/chipKIT-WiFire-3Mbps.hex).

When using this bootloader set the board type to the 3Mbps variant in board selection.

In order to program the WiFire (non-Flow version) you will have to <br> change `chipkit_WiFire.upload.speed` to `300000000` in the config file <br> found in `hardware/pic32/variants/WiFire/boards.txt`

<hr>

## Decreasing wait times on the FTDI chip

The next thing we can do is decrease the latency timer in the FTDI chip. The WiFire uses a FTDI chip between the USB and the WiFire for serial communication. 

### Background
The FTDI chip has an internal buffer of 62 bytes (+ 2 status bytes) which it fills before sending out on its UART channels (both RX and TX). 
If this buffer does not fill within a timeout (default 16ms) the chip sends the buffer anyway as the data still needs to arrive.

The use of the buffer significantly reduces overhead on the USB as instead of needing to generate a USB packet for almost every byte the transmission of data can be broken into blocks of 62 bytes (or 16ms worth of data, whichever happens first). This packaging of the data is fine for *sending* the program to the chip as the blocks of data sent are several times greater than 62 bytes so do not spend as much time waiting. 

The issue stems from the fact that STK500 also requires verification of each data block sent. These verification packets are less than 62 bytes (as STK500 does not optimise for use with FTDI chips) and therefore sit in the receive buffer for 16ms before being sent back to the programming computer. Because of this avrdude must wait 16ms beyond what is required before sending the next block of data.

### What we can change
By decreasing the timeout we can reduce how long avrdude has to wait for the verification.
Doing so will increase the USB overhead making the programmer do more work and potentially even lose data.
To decrease the timeout in windows open the device manager, go to the Ports (COM and LPT), choose the FTDI USB Serial Port and change the timeout under the port's `Properties > Port Settings > Advanced > BM Options`

<hr>

A final tweak would be to disable verification in avrdude. This requires modifying MPIDE, either recompiling, changing the java bytecode or making a wrapper avrdude for MPIDE to call instead that strips off the flags and calls the real avrdude. This method is not worth it and only shaves off a couple of seconds.