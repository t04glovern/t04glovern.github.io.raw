---
layout: post
title: "Arduino Projects - USB to Serial interfacing + SD Read/Write"
date: "2016-01-06"
comments: true
description: Working through some interfacing with a USB to Serial module while also trying to learn a little bit about SD card reading and writing
share: true
author: nathan
tags:
 - arduino
 - sdcard
 - serial
 - usb
---

## Introduction

This week I picked up some new toys to play with and decided I might as well document my experiences as I try to get a better understanding of how to use my new modules. I got a USB to Serial module and an `Ethernet shield`; which also has a `SD card reader` attached. During this tutorial I'll go over the FT232RL chip in detail and finish up with some information on reading and writing to an SD card.

I have a feeling this process might be harder than it lets on, so lets get started.

## USB to Serial module

![USB to Serial Board]({{ site.url }}/images/posts/2016.01.06/arduino-ft232-board.png)

To start with lets have a look at the USB to Serial module. It is a 6-pin serial port module running a `FT232` chip. Before we have a look at the PINs themselves, lets first confirm that we can pick up the module in our operating system.

If you plugin the module, and you haven't dealt with one like this before; there is a good chance you'll receive and error similar to the following indicating that there aren't any acceptable drivers available on your system to drive the module.

![USB to Serial Error 1]({{ site.url }}/images/posts/2016.01.06/arduino-usb-serial-driver-error-1.png)

Navigate to device manager and you'll notice a similar warning on the new device

![USB to Serial Error 2]({{ site.url }}/images/posts/2016.01.06/arduino-usb-serial-driver-error-2.png)

In order to get the correct drivers for your chip, I would recommend googling your ICs module number followed by something like `"usb serial drivers"` in order to track down the correct ones for yourself. For mine however, I know it's a FT232 so I'm able to just download the correct Operating system supported drivers from [here](http://www.ftdichip.com/Drivers/VCP.htm).

Once downloaded, you can go through the sets of manually adding the new drivers by right clicking on the troublesome device in `Device manager` and selecting `Update Driver Software`

![USB to Serial Error 3]({{ site.url }}/images/posts/2016.01.06/arduino-usb-serial-driver-error-3.png)

Then select `Browse for driver software on your computer` and point to the directory we just downloaded containing the new drivers (normally in a .ini format).

![USB to Serial Error 4]({{ site.url }}/images/posts/2016.01.06/arduino-usb-serial-driver-error-4.png)

Once the drivers are loaded successfully you'll see the device appear under the `COM ports` section of the device view.

![USB to Serial Error 5]({{ site.url }}/images/posts/2016.01.06/arduino-usb-serial-driver-error-5.png)

Awesome, we know that our device is actually going to function! Lets move on to interfacing with the module.

## FT232RL Pins

![FT232RL Board]({{ site.url }}/images/posts/2016.01.06/arduino-ft232rl-board.jpg)

The FT232RL chip is a `28pin SSOP IC`. The specifics of the chip are outlined in the datasheet [here](http://www.ftdichip.com/Support/Documents/DataSheets/ICs/DS_FT232R.pdf).

The Pin on the IC can be viewed below. I've tried my best to summarize.

![FT232RL Pinout]({{ site.url }}/images/posts/2016.01.06/arduino-ft232rl-pinouts.png)

1. USB Interface Group

* `Pin 15 (USBDP)` - USB Data Signal Plus, incorporating internal series resistor and 1.5kΩ pull up resistor to 3.3V.

* `Pin 16 (USBDM)` - USB Data Signal Minus, incorporating internal series resistor.

2. Power and Ground Group

* `Pin 4 (VCCIO)` - +1.8V to +5.25V supply to the UART Interface and CBUS group pins

* `Pin 7, 18, 21 (GND)` - Group Supply Pins

* `Pin 17 (3V3OUT)` - +3.3V output from integrated LDO regulator.

* `Pin 20 (VCC)` - +3.3V to +5.25V supply to the device core.

* `Pin 25 (AGND)` - Device analogue ground supply for internal clock multiplier.

Miscellaneous Signal Group

* `Pin 8, 24 (NC)` - No internal connection

* `Pin 19 (RESET#)` - Active low reset pin. Can be used by an external device to reset the FT232R. If not required can be left unconnected, or pulled up to VCC.

* `Pin 26 (TEST)` - Puts the device into IC test mode. Must be tied to GND for normal operation, otherwise the device will appear to fail.

* `Pin 27 (OSCI)` - Input 12MHz Oscillator Cell. Optional and can be left unconnected for normal operation.

* `Pin 28 (OSCO)` - Output from 12MHz Oscillator cell. Optional for normal operation if internal Oscillator is used.

UART Interface and CUSB Group

* `Pin 1 (TXD)` - Transmit Asynchronous Data Output.

* `Pin 2 (DTR#)` - Data Terminal Ready Control Output / Handshake Signal.

* `Pin 3 (RTS#)` - Request to Send Control Output / Handshake Signal.

* `Pin 5 (RXD)` - Receiving Asynchronous Data Input.

* `Pin 6 (RI#)` - Ring Indicator Control Input. When remote wake up is enabled in the internal EEPROM taking RI# low (20ms active low pulse) can be used to resume the PC USB host controller from suspend.

* `Pin 9 (DSR#)` - Data Set Ready Control Input / Handshake Signal.

* `Pin 10 (DCD)` - Data Carrier Detect Control Input.

* `Pin 11 (CTS#)` - Clear To Send Control Input / Handshake Signal.

* `Pin 12 (CBUS4)` - Configurable CBUS output only Pin.

* `Pin 13 (CBUS2)` - Configurable CBUS I/O Pin.

* `Pin 14 (CBUS3)` - Configurable CBUS I/O Pin.

* `Pin 22 (CBUS1)` - Configurable CBUS I/O Pin.

* `Pin 23 (CBUS0)` - Configurable CBUS I/O Pin.

## What can we use this for?
