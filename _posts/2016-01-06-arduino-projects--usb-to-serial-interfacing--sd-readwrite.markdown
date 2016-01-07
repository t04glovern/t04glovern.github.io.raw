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

`This week I picked up some new toys to play with and decided I might as well document my experiences as I try to get a better understanding of how to use my new modules. I got a USB to Serial module` and an `Ethernet shield`; which also has a `SD card reader` attached. My goal is to be able to send serial data down the USB interface and have it then saved into a new file on the inserted SD card.
`
I have a feeling this process might be harder than it lets on, so lets get started.

## USB to Serial module

![USB to Serial Board]({{ site.url }}/images/posts/arduino-ft232-board.png)

To start with lets have a look at the USB to Serial module. It is a 6-pin serial port module running a `FT232` chip. Before we have a look at the PINs themselves, lets first confirm that we can pick up the module in our operating system.

If you plugin the module, and you haven't dealt with one like this before; there is a good chance you'll receive and error similar to the following indicating that there aren't any acceptable drivers available on your system to drive the module.

![USB to Serial Error 1]({{ site.url }}/images/posts/arduino-usb-serial-driver-error-1.png)

Navigate to device manager and you'll notice a similar warning on the new device

![USB to Serial Error 2]({{ site.url }}/images/posts/arduino-usb-serial-driver-error-2.png)

In order to get the correct drivers for your chip, I would recommend googling your ICs module number followed by something like "usb serial drivers" in order to track down the correct ones for yourself. For mine however, I know it's a FT232 so I'm able to just download the correct Operating system supported drivers from [here](http://www.ftdichip.com/Drivers/VCP.htm).

Once downloaded, you can go through the sets of manually adding the new drivers by right clicking on the troublesome device in `Device manager` and selecting `Update Driver Software`

![USB to Serial Error 3]({{ site.url }}/images/posts/arduino-usb-serial-driver-error-3.png)

Then select `Browse for driver software on your computer` and point to the directory we just downloaded containing the new drivers (normally in a .ini format).

![USB to Serial Error 4]({{ site.url }}/images/posts/arduino-usb-serial-driver-error-4.png)
