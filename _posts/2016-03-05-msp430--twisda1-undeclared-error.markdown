---
layout: post
title: "MSP430 - 'TWISDA1' undeclared error"
date: "2016-03-05"
comments: true
description: Instructions to resolve 'TWISDA1' undeclared errors in Energia 17
share: true
author: nathan
tags:
 - msp430
 - energia
---

## Problem

***

When trying to compile code for the MSP430 Experimental board (specifically the MSP-EXP430FR5739) in Energia version 0101E0017 the following error stream is presented:

{% highlight bash %}
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c: In function 'i2c_sw_init':
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:43:16: error: 'TWISDA1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:43:16: note: each undeclared identifier is reported only once for each function it appears in
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:44:16: error: 'TWISCL1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c: In function 'i2c_sw_start':
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:84:11: error: 'TWISDA1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:86:11: error: 'TWISCL1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c: In function 'i2c_sw_txByte':
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:99:15: error: 'TWISDA1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:103:13: error: 'TWISCL1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c: In function 'i2c_sw_ack':
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:122:12: error: 'TWISCL1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c: In function 'i2c_sw_rxByte':
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:132:12: error: 'TWISDA1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:136:15: error: 'TWISCL1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c: In function 'i2c_sw_stop':
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:157:11: error: 'TWISDA1' undeclared (first use in this function)
C:\Program Files (x86)\Energia\hardware\msp430\cores\msp430\twi_sw.c:159:11: error: 'TWISCL1' undeclared (first use in this function)
{% endhighlight bash %}

The issues occurs because the the I2C SW library loads but isn't included explicitely so the sketch won't load (yay c)

***

## Resolution

***

The fix is documented [here](https://github.com/energia/Energia/commit/827e338d22f57e03f21ffb5a064c271498d983b6) and is available for people who clone their Energia from git instead of using the installation binary.

You have two options:

1. Clone/Update from the [GitHub repo](https://github.com/energia/Energia)
2. Make the changes yourself!

Your choice...

***

#### Option 1 - Clone/Update

***

Ensure you have [git](http://www.git-scm.com/) installed and clone the repo to a desired location on your harddrive

{% highlight bash %}
git clone --recursive https://github.com/energia/Energia.git
{% endhighlight bash %}

If you already have a clone of Energia then:

{% highlight bash %}
git submodule update --init --recursive
git submodule sync
cd emt
git pull
cd ..
{% endhighlight bash %}

***

#### Option 2 - Update yourself

***

Navigate to the following subdirectory of the Energia folder and open twi_sw.c

{% highlight bash %}
Energia\hardware\msp430\cores\msp430\twi_sw.c
{% endhighlight bash %}

On line 26 you should see your `#include "twi_sw.h"`. Just below that add the following line:

{% highlight c %}
#if DEFAULT_I2C == -1 /* indicates SW I2C on pseudo module 1 */
{% endhighlight c %}

![Line Change 1]({{ site.url }}/images/posts/2016.03.05/energia-line-change-1.png)

On the last line in the file (just after the last bracket) add your endif statement

{% highlight bash %}
#endif // #if DEFAULT_I2C = -1 /* indicates SW I2C on pseudo module 1 */
{% endhighlight bash %}

![Line Change 2]({{ site.url }}/images/posts/2016.03.05/energia-line-change-2.png)

***

## Results

***

After making the changes above you should be able to push your sketch over to your MSP430 without any issues
