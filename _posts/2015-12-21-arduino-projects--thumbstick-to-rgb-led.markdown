---
layout: post
title: "Arduino Projects - Thumbstick to RGB LED"
date: "2015-12-21"
tags:
 - arduino
 - electronics
---

## Introduction

We just had a [Jaycar](http://www.jaycar.com.au/) open in our area and I've taken it upon myself to drop in and pick up some in-expensive components every now and then in preparation for some of the `Arduino/Pi` based units I'll be taking next semester when Uni goes back.

I was able to pick up a relatively cheap analog thumbstick today and I came up with the idea to have it control one of the RGB LED units I had sitting around gathering dust at home. I didn't want to over complicate the project, as I didn't have a huge amount of experience with the platform and was honestly going to be happy just getting analog serial readings from the thumbstick showing in the `Arduino IDE`.

## Part List

The following parts were used throughout the build of this project:

{% highlight bash %}
1x Arduino UNO
1x Analog Thumbstick
1x RGB LED Module
3x 220Ω Resistor
10x Jumper wires
{% endhighlight bash %}

The final schematic I'll be referring to throughout the design process is shown below. Output pins on the Thumbstick module might vary on different units, so just make sure your unit lines up with the four important pins I've used in my build.

![Thumbstick RGB Circuit]({{ site.url }}/images/posts/arduino-thumbstick-rgb-led.png)

## Reading input from the Thumbstick

To start with I wanted to make sure that I could easily read from logical values from the thumbstick. In order to do this I connected the `VRx` and `VRy` lines to the Analog IN Pins `AO` and `A1` on the `Arduino` board.

I then ran 5Volts and a grounding line to the other two pins on the thumbstick. The following code was then used to read from the Analog PINs and output the results to the Serial console.

{% highlight java %}
// Analog PIN Declarations
const int sensorPinX = A0;
const int sensorPinY = A1;

void setup() {
  // open a serial port
  Serial.begin(9600);
}

void loop() {
  // Read sensor values
  int sensorXValue = analogRead(sensorPinX);
  int sensorYValue = analogRead(sensorPinY);

  // Print results to console
  Serial.print("X Axis Value: ");
  Serial.println(sensorXValue);
  Serial.print("Y Axis Value: ");
  Serial.println(sensorYValue);
}
{% endhighlight java %}

Writing this code to the board and executing immediately gave me positive results spilling
