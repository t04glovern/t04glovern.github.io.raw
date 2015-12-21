---
layout: post
title: "Arduino Projects - Thumbstick to RGB LED"
date: "2015-12-21"
tags:
 - arduino
 - electronics
---

## Introduction

![Thumbstick RGB Circuit]({{ site.url }}/images/posts/arduino-thumbstick-rgb-led.png)

{% highlight java %}
// Analog PIN Declarations
const int sensorPinX = A0;
const int sensorPinY = A1;

// Digital/PMW PIN Declarations
const int redLEDPin = 11;
const int greenLEDPin = 10;
const int blueLEDPin = 9;

// RGB Variable Declarations
int redValue = 0;
int greenValue = 0;
int blueValue = 0;

// Sensor High/Low Declarations
int sensorXLow = 0;
int sensorXHigh = 1023;
int sensorYLow = 0;
int sensorYHigh = 1023;

void setup() {
  // open a serial port
  Serial.begin(9600);

  // set LED pins to output
  pinMode(redLEDPin,OUTPUT);
  pinMode(greenLEDPin,OUTPUT);
  pinMode(blueLEDPin,OUTPUT);
}

void loop() {
  // Read sensor values
  int sensorXValue = analogRead(sensorPinX);
  int sensorYValue = analogRead(sensorPinY);

  // Calibrate High/Low X
  if (sensorXValue > sensorXHigh){
    sensorXHigh = sensorXValue;
  }
  if (sensorXValue < sensorXLow){
    sensorXLow = sensorXValue;
  }

  // Calibrate High/Low Y
  if (sensorYValue > sensorYHigh){
    sensorYHigh = sensorYValue;
  }
  if (sensorYValue < sensorYLow){
    sensorYLow = sensorYValue;
  }

  // Calculate and map new values
  int valueX = map(sensorXValue,sensorXLow,sensorXHigh, 0, 255);
  int valueY = map(sensorYValue,sensorYLow,sensorYHigh, 0, 255);
  int valueMix = map((sensorXValue+sensorYValue),0,1024, 0, 255);

  // Print results to console
  Serial.print("X Axis Value: ");
  Serial.println(valueX);
  Serial.print("Y Axis Value: ");
  Serial.println(valueY);

  // Write values out to RGB LED
  analogWrite(redLEDPin, valueX);
  analogWrite(greenLEDPin, valueY);
  analogWrite(blueLEDPin, valueMix);

  // Delay allowing system to process
  delay(30);
}
{% endhighlight java %}
