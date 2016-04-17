---
layout: post
title: "CCNA3 - OSPF Practice Skills Assessment"
date: "2016-04-18"
comments: true
description: I walkthrough the general process for sitting the CCNA3 OSPF Skills Assessment
share: true
author: nathan
tags:
 - ccna3
 - ospf
---

# Overview

The following are the skills covered in the following assessment:

* Configuration of initial device settings
* IPv4 address assignment and configuration
* Configuration and addressing of device interfaces
* Configuration of the OSPFv2 routing protocol
* Configuration of a default route
* Configuration of ACL to limit device access
* Configuration of switch management settings including SSH
* Configuration of port security
* Configuration of unused switch ports according to security best practices
* Configuration of RPVST+
* Configuration of  EtherChannel 
* Configuration of a router as a DHCP server
* Configuration of VLANs and trunks
* Configuration of routing between VLANs

# Initial Information

#### **Network Diagram**
![Addressing Table Complete]({{ site.url }}/images/posts/2016.04.18/ospf-network-diagram.png)

#### **Addressing table**
![Addressing Table]({{ site.url }}/images/posts/2016.04.18/ospf-address-table.png)

#### **VLAN Switch Port Assignment Table**
![VLAN Switch Port Table]({{ site.url }}/images/posts/2016.04.18/vlan-switch-port-assignment-table.png)

#### **Port Channel Groups**
![Port Channel Groups]({{ site.url }}/images/posts/2016.04.18/port-channel-groups.png)

# Step 1: Plan the Addressing

#### **Task:**
Determine the IP addresses that you will use for the required interfaces on the devices and LAN hosts. Follow the configuration details provided in the Addressing Table.

#### **How:**
This part requires and understanding of the addressing constants that make up a normal network separated by subnets. Let's take a look at the first entry we are asked to find. We are asked to pick `any address in the network 192.168.100.20/30`.

A Typical network range has two addresses that are not suitable as host addresses, these are the First and the Last addresses are corespond to the Subnet ID and Broadcast address. So with our network we know that 192.168.100.20 will be the Subnet ID so our First usable host address will be `192.168.100.21/30`

The next issue we run into is working out how many host addresses are available in the subnet. To explain this process I'm going to draw out 32bits representing the 4 octets of 8bits that make up an IPv4 address.
{% highlight bash %}
[00000000] . [00000000] . [00000000] . [00000000]
{% endhighlight bash %}

Now fill up all the slots that correspond to 30 of the 32 bits
{% highlight bash %}
[11111111] . [11111111] . [11111111] . [11111100]
{% endhighlight bash %}

This leaves a total of 2 bits for our hosts to use.
{% highlight bash %}
BIT     ADDRESS
00      192.168.100.20      Subnet ID
01      192.168.100.21      Host
10      192.168.100.22      Host
11      192.168.100.23      Broadcast Address
{% endhighlight bash %}

Using the information above will normally allow you to easily work through the table of addresses and pick suitable candicates. Below is the table I will use for the rest of this post.

#### **Addressing table - Complete**
![Addressing Table Complete]({{ site.url }}/images/posts/2016.04.18/ospf-address-table-complete.png)

# Step 2: Configure Building 1

#### **Task:**
Configure Building 1 with initial settings:

* Configure the router host name: Bldg-1. This value must be entered exactly as it appears here.
* Prevent the router from attempting to resolve command line entries to IP addresses.
* Protect device configurations from unauthorized access with an encrypted secret password.
* Secure the router console and remote access lines.
* Prevent system status messages from interrupting console output.
* Configure a message-of-the-day banner.
* Encrypt all clear text passwords.

#### **How:**
* Configure the router host name: Bldg-1. This value must be entered exactly as it appears here.

{% highlight cmd %}
Router(config)#hostname Bldg-1
{% endhighlight cmd %}

* Prevent the router from attempting to resolve command line entries to IP addresses.

{% highlight cmd %}
Bldg-1(config)#no ip domain lookup
{% endhighlight cmd %}

* Protect device configurations from unauthorized access with an encrypted secret password.

{% highlight cmd %}
Bldg-1(config)#enable secret class
{% endhighlight cmd %}

* Secure the router console and remote access lines.

{% highlight cmd %}
Bldg-1(config)#line console 0
Bldg-1(config-line)#password cisco
Bldg-1(config-line)#login
Bldg-1(config-line)#line vty 0 4
Bldg-1(config-line)#password cisco
Bldg-1(config-line)#login
Bldg-1(config-line)#line aux 0
Bldg-1(config-line)#password cisco
Bldg-1(config-line)#login
{% endhighlight cmd %}

* Prevent system status messages from interrupting console output.

{% highlight cmd %}
Bldg-1(config)#line console 0
Bldg-1(config-line)#logging synchronous
{% endhighlight cmd %}

* Configure a message-of-the-day banner.

{% highlight cmd %}
Bldg-1(config)#banner motd "Authorized Access Only"
{% endhighlight cmd %}

* Encrypt all clear text passwords.

{% highlight cmd %}
Bldg-1(config)#service password-encryption
{% endhighlight cmd %}

# Step 3: Configure the Router Interfaces

#### **Task:**
Configure the interfaces of all routers for full connectivity with the following:

* IP addressing
* Descriptions for serial interfaces.
* Configure DCE settings where required. Use a rate of 128000.

#### **How:**
* IP addressing

{% highlight cmd %}

{% endhighlight cmd %}

* Descriptions for serial interfaces.

{% highlight cmd %}

{% endhighlight cmd %}

* Configure DCE settings where required. Use a rate of 128000.

{% highlight cmd %}

{% endhighlight cmd %}

* The Ethernet subinterfaces on Site 2 will configured later in this assessment.

{% highlight cmd %}

{% endhighlight cmd %}
