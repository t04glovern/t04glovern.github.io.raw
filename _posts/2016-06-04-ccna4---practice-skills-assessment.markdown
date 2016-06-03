---
layout: post
title: CCNA4 - Practice Skills Assessment
date: "2016-06-04"
comments: true
description: A general overview of skills required for the CCNA4 Practical Assessment 
share: true
author: nathan
tags:
 - ccna4
 - nat
---

## <center>Contents</center>

---

1. **Initialize Devices**
  * 1.1. Initialize and reload routers
2. **Configure Device Basic Settings**
  * 2.1. Configure PCs
  * 2.2. Configure R1
3. **Configure PPP Connections**
4. **Configure NAT**
5. **Monitor the Network**
6. **Configure Frame Relay**
7. **Configure a GRE VPN Tunnel**

`NOTE`: This guide is based on the following: https://www.youtube.com/watch?v=0vyxmBJ1lxc

---

## <center>Topology</center>

---

![Topology]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-topology.png){:.center-image}

---

## <center>1. Initialize Devices</center>

---

#### **Initialize and reload routers**

Erase the startup-config file on all routers.

{% highlight bash %}
R1>enable
R1#erase startup-config

Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]

Erase of nvram: complete
%SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram
{% endhighlight bash %}

Reload all routers.

{% highlight bash %}
R1#reload

Proceed with reload? [confirm]
{% endhighlight bash %}

---

## <center>2. Configure Device Basic Settings</center>

---

#### **Configure PCs**

Configure static IPv4 address information on PC-A.

![Topology]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-pc-a.png){:.center-image}

Configure static IPv4 address information on PC-B.

![Topology]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-pc-b.png){:.center-image}

Configure static IPv4 address information on PC-C.

![Topology]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-pc-c.png){:.center-image}

#### **Configure R1**

Disable DNS lookup



Router name



Encrypted privileged EXEC password



Console access password



Telnet access password



Encrypt the plain text passwords



MOTD banner


Configure G0/0


