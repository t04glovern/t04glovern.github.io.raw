---
layout: post
title: "Arduino Projects - Shift Registers"
date: "2016-01-03"
comments: true
description: Investigating Shift Registers with Arduino
share: true
author: nathan
tags:
 - arduino
 - shift register
---

## Introduction

I've been looking into some projects involving an `LED matrix`; but hit a few road blocks due to issues with the sheer number of points and LEDs I need to control. I've always known about the existence of shift registers and their ability to control many LEDs and other components without needing separate pins to drive each small bit of my circuits.

I picked up a kit recently that had a [SN74HC595N Shift Register](http://www.digikey.com/product-detail/en/SN74HC595N/296-1600-5-ND/277246) in it and thought I'd best learn how they work so that my future projects aren't so wire heavy.

## Understanding Shift Registers

A `shift register` is a device that accepts a stream of serial bits and simultaneously output the values of those bits onto parallel I/O pins. This comes in handy when trying to drive a large numbers of LEDs or seven-segment displays. The diagram below illustrates how various inputs affect the outputs.

![Shift Register Basic]({{ site.url }}/images/posts/arduino-shift-reg-basic.png)

The inputs are three serial communication lines that connect to the shift register from the Arduino. The eight circles represent LEDs connected to eight outputs of the shift register.

Say you wanted sufficient data to control eight LEDs digitally; you need to find a way to transmit 8 total bits of information. If we wanted to turn on eight LEDs with eight digital outputs; all the bits would be transmitted on independent I/O pins.

We are going to be focusing on serial in, parallel out (SIPO) shift registers. These devices allow us to `clock in` multiple bytes of data serially, and output them from the shift register all at once in a parallel fashion. You can also do cool things like chaining multiple shift registers together and have them all controlled by a single set of three pins.

Take a look at the pin-out diagram for our Shift register below

![Shift Register Pinout]({{ site.url }}/images/posts/arduino-shift-pinout.png)

1. `Q(A) -> Q(H)` - Represent the eight parallel outputs from the shift register. These are the same outputs shown in the first diagram.

2. `VCC` - Connected to 5V

3. `GND` - Shared grounding pin

4. `SER` - The equivalent of the `DATA pin` shown in the first diagram. This is the pin where you will feed in the eight sequential bit values to set the values of the parallel output.

5. `SRCLK` - The equivalent of the `CLOCK pin` shown in the first diagram. Every time the pin goes high, the values in the shift register shift by 1 bit. It will be pulsed eight times to pull in all the data that you are sending on the data pin.

6. `RCLK` - The equivalent of the `LATCH pin` shown in the first diagram. Also known as the `register clock pin`, the latch pin is used to `commit` the recently shifted serial values to the parallel outputs all at once. This pins allows you to sequentially shift data into the chip and have all the values show up on the parallel outputs at the same time.

7. `OE` - Output Enable, the bar over the top indicates that is active low; meaning that when the pin is held low, the output will be enabled. When it is high the output will be disabled.

8. `SRCLR` - Serial Clear pin, again the bar means that is is also an active low. When pulled low, it empties the contents of the shift register.

## How the Shift Register works

A shift register is a synchronous device; it only acts on the rising edge of the clock signal. Every time the clock signal transitions from low to high, all the values currently stored in the eight output registers are shifted over one position (the last one is either discarded or output on the Q(H)' pin if you are cascading registers).

Simultaneously, the value currently on the DATA input is shifted into the first position. By doing this eight times, the present values are shifted out and the new values are shifted into the register. The LATCH pin is set high at the end of the cycle to make the newly shifted values appear on the outputs.

Below depicts the process we would take if we wanted to set every other LED to the ON state (`Q(A)`, `Q(C)`, `Q(E)`, `Q(G)`). Represented in binary, we want the output of the parallel pins on the shift register to look like this: `10101010`.

![Shift Register Process]({{ site.url }}/images/posts/arduino-shift-process.png)

First, the LATCH pin is set low so that the current LED states are not changed while new values are shifted in. Then, the LED states are shifted into the registers in order on the CLOCK edge from the DATA line. After all the values have been shifted in, the LATCH pin is set high again, and the values are outputted from the shift registers.

## Shifting Serial Data from the Arduino

Now that we understand how the Shift Register works, we can write Arduino code to control the shift register. Luckily there is already a library for controlling this type of system called [ShiftOut](https://www.arduino.cc/en/Reference/ShiftOut).

`ShiftOut()` is setup to shift out a byte of data at a time. It can be told to start shifting from either the most or least significant bit and uses the following syntax with the listed arguments:

{% highlight c %}
shiftOut(dataPin, clockPin, bitOrder, value)
{% endhighlight c %}

* `dataPin` - Specifies the data pin (SER) number

* `clockPin` - Specifies the clock pin (SRCLK) number

* `bitOrder` - Set to either MSBFIRST or LSBFIRST to define what order we want the bits read into the shift register

* `value` - The value to shift out. If you are feeding in a binary number similar to the example above; it must be prefixed with a capital `B` in order to alert the Arduino IDE to interpret the following numbers as binary rather than an integer. `eg. B10101010`

In order to wire up this circuit I
