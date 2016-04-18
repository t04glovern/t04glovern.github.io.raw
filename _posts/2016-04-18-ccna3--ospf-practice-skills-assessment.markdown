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

## Overview

***

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

***

## Initial Information

***

#### **Network Diagram** (diagram uses slightly different naming convention, same topology)
![Addressing Table Complete]({{ site.url }}/images/posts/2016.04.18/ospf-network-diagram.png)

#### **Addressing table**
![Addressing Table]({{ site.url }}/images/posts/2016.04.18/ospf-address-table.png)

#### **VLAN Switch Port Assignment Table**
![VLAN Switch Port Table]({{ site.url }}/images/posts/2016.04.18/vlan-switch-port-assignment-table.png)

#### **Port Channel Groups**
![Port Channel Groups]({{ site.url }}/images/posts/2016.04.18/port-channel-groups.png)

***

## Step 1: Plan the Addressing

***

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

***

## Step 2: Configure Building 1

***

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

{% highlight bash %}
Router(config)#hostname Bldg-1
{% endhighlight bash %}

* Prevent the router from attempting to resolve command line entries to IP addresses.

{% highlight bash %}
Bldg-1(config)#no ip domain lookup
{% endhighlight bash %}

* Protect device configurations from unauthorized access with an encrypted secret password.

{% highlight bash %}
Bldg-1(config)#enable secret class
{% endhighlight bash %}

* Secure the router console and remote access lines.

{% highlight bash %}
Bldg-1(config)#line console 0
Bldg-1(config-line)#password cisco
Bldg-1(config-line)#login
Bldg-1(config-line)#line vty 0 4
Bldg-1(config-line)#password cisco
Bldg-1(config-line)#login
Bldg-1(config-line)#line aux 0
Bldg-1(config-line)#password cisco
Bldg-1(config-line)#login
{% endhighlight bash %}

* Prevent system status messages from interrupting console output.

{% highlight bash %}
Bldg-1(config)#line console 0
Bldg-1(config-line)#logging synchronous
{% endhighlight bash %}

* Configure a message-of-the-day banner.

{% highlight bash %}
Bldg-1(config)#banner motd "Authorized Access Only"
{% endhighlight bash %}

* Encrypt all clear text passwords.

{% highlight bash %}
Bldg-1(config)#service password-encryption
{% endhighlight bash %}

***

## Step 3: Configure the Router Interfaces

***

#### **Task:**
Configure the interfaces of all routers for full connectivity with the following:

* IP addressing
* Descriptions for serial interfaces.
* Configure DCE settings where required. Use a rate of 128000.

#### **How:**
* IP addressing

{% highlight bash %}
Bldg-1(config)#int s0/0/0
Bldg-1(config-if)#ip address 192.168.100.22 255.255.255.252
Bldg-1(config-if)#no shutdown

Bldg-1(config)#int g0/0
Bldg-1(config-if)#ip address 192.168.8.1 255.255.255.0
Bldg-1(config-if)#no shutdown

Bldg-1(config)#int g0/1
Bldg-1(config-if)#ip address 192.168.9.1 255.255.255.0
Bldg-1(config-if)#no shutdown

Main(config)#int s0/0/0
Main(config-if)#ip address 192.168.100.21 255.255.255.252
Main(config-if)#no shutdown

Main(config)#int s0/0/1
Main(config-if)#ip address 192.168.100.37 255.255.255.252
Main(config-if)#no shutdown

Main(config)#int s0/1/0
Main(config-if)#ip address 203.0.113.18 255.255.255.248
Main(config-if)#no shutdown

Bldg-2(config)#int s0/0/1
Bldg-2(config-if)#ip address 192.168.100.38 255.255.255.252
Bldg-2(config-if)#no shutdown

Bldg-2(config)#int g0/1
Bldg-2(config-if)#no shutdown
{% endhighlight bash %}

* Descriptions for serial interfaces.

{% highlight bash %}
Main(config)#int s0/0/0
Main(config-if)#description 2-Building1

Main(config)#int s0/0/1
Main(config-if)#description 2-Building2

Main(config)#int s0/1/0
Main(config-if)#description 2-INTERNET
{% endhighlight bash %}

* Configure DCE settings where required. Use a rate of 128000.

{% highlight bash %}
Bldg-1(config)#int s0/0/0
Bldg-1(config-if)#clock rate 128000

Main(config)#int s0/0/1
Main(config-if)#clock rate 128000
{% endhighlight bash %}

***

## Step 4: Configure inter-VLAN routing on Building 2

***

#### **Task:**
Configure router Building 2 to route between VLANs using information in the Addressing Table and VLAN Switch Port Assignment Table. The VLANs will be configured on the switches later in this assessment.

* Do not route the VLAN 99 network.

#### **How:**
{% highlight bash %}
Bldg-2(config)#int g0/1.2
Bldg-2(config-subif)#encapsulation dot1Q 2
Bldg-2(config-subif)#ip address 10.10.2.1 255.255.255.0

Bldg-2(config-subif)#int g0/1.4
Bldg-2(config-subif)#encapsulation dot1Q 4
Bldg-2(config-subif)#ip address 10.10.4.1 255.255.255.0

Bldg-2(config-subif)#int g0/1.8
Bldg-2(config-subif)#encapsulation dot1Q 8
Bldg-2(config-subif)#ip address 10.10.8.1 255.255.255.0

Bldg-2(config-subif)#int g0/1.15
Bldg-2(config-subif)#encapsulation dot1Q 15
Bldg-2(config-subif)#ip address 10.10.15.1 255.255.255.0

Bldg-2(config-subif)#int g0/1.25
Bldg-2(config-subif)#encapsulation dot1Q 25
Bldg-2(config-subif)#ip address 10.10.25.1 255.255.255.0
{% endhighlight bash %}

***

## Step 5: Configure Default Routing

***

#### **Task:**
On Main, configure a default route to the Internet. Use the exit interface argument.

#### **How:**
{% highlight bash %}
Main(config)#ip route 0.0.0.0 0.0.0.0 s0/1/0
{% endhighlight bash %}

***

## Step 6: Configure OSPF Routing

***

#### **Task:**
1. On all routers:
  * Configure multiarea OSPFv2 to route between all internal networks. Use a process ID of 1.
  * Use the area numbers shown in the topology.
  * Use the correct wild card masks for all network statements.
  * You are not required to route the manage network on Building 2.
  * Prevent routing updates from being sent to the LANs.

2. On the Main router:
  * Configure multiarea OSPFv2 to distribute the default route to the other routers.

#### **How:**
* Configure multiarea OSPFv2 to route between all internal networks. Use a process ID of 1.
* Use the area numbers shown in the topology.
* Use the correct wild card masks for all network statements.
* You are not required to route the manage network on Building 2.

Calculating the wildcard for each OSPF route is done by looking again at the subnet mask bit. lets use the 192.168.100.20/30 network. The following shows what a wild card mask for 0.0.0.0 or /32 would look like

{% highlight bash %}
[00000000] . [00000000] . [00000000] . [00000000]
{% endhighlight bash %}

If we have 30 bits instead we're left with two bits

{% highlight bash %}
[00000000] . [00000000] . [00000000] . [00000011]
{% endhighlight bash %}

This gives us a wild card of 0.0.0.3. Similarly you can look at what a /24 networks bit pattern looks like below.

{% highlight bash %}
[00000000] . [00000000] . [00000000] . [11111111]
{% endhighlight bash %}

/24's wild card mask is 0.0.0.255.

Using this information we can setup the various networks on each of the routers

{% highlight bash %}
Bldg-1(config)#router ospf 1
Bldg-1(config-router)#network 192.168.100.20 0.0.0.3 area 0
Bldg-1(config-router)#network 192.168.8.0 0.0.0.255 area 1
Bldg-1(config-router)#network 192.168.9.0 0.0.0.255 area 1

Main(config)#router ospf 1
Main(config-router)#network 192.168.100.20 0.0.0.3 area 0
Main(config-router)#network 192.168.100.36 0.0.0.3 area 0

Bldg-2(config)#router ospf 1
Bldg-2(config-router)#network 192.168.100.36 0.0.0.3 area 0
Bldg-2(config-router)#network 10.10.2.0 0.0.0.255 area 2
Bldg-2(config-router)#network 10.10.4.0 0.0.0.255 area 2
Bldg-2(config-router)#network 10.10.8.0 0.0.0.255 area 2
Bldg-2(config-router)#network 10.10.15.0 0.0.0.255 area 2
{% endhighlight bash %}

* Prevent routing updates from being sent to the LANs.

{% highlight bash %}
Bldg-1(config)#router ospf 1
Bldg-1(config-router)#passive-interface GigabitEthernet0/0
Bldg-1(config-router)#passive-interface GigabitEthernet0/1

Main(config)#router ospf 1
Main(config-router)#passive-interface Serial0/1/0

Bldg-2(config)#router ospf 1
Bldg-2(config-router)#passive-interface GigabitEthernet0/1
Bldg-2(config-router)#passive-interface g0/1.2
Bldg-2(config-router)#passive-interface g0/1.4
Bldg-2(config-router)#passive-interface g0/1.8
Bldg-2(config-router)#passive-interface g0/1.15
{% endhighlight bash %}

* Configure multiarea OSPFv2 (Main) to distribute the default route to the other routers.

{% highlight bash %}
Main(config)#router ospf 1
Main(config-router)#default-information originate
{% endhighlight bash %}

***

## Step 7: Customize Multiarea OSPFv2

***

#### **Task:**
Customize multiarea OSPFv2 by performing the following configuration tasks:

1. Set the bandwidth of all serial interfaces to 128 kb/s.

2. Configure OSPF router IDs as follows:
  * Building 1: 1.1.1.1
  * Main: 2.2.2.2
  * Building 2: 3.3.3.3
  * The configured router IDs should be in effect on all three routes.

3. Configure the OSPF cost of the link between Building 1 and Main to 7500.

#### **How:**
Set the bandwidth of all serial interfaces to 128 kb/s.

{% highlight bash %}
Bldg-1(config)#int s0/0/0
Bldg-1(config-if)#bandwidth 128

Main(config)#int s0/0/0
Main(config-if)#bandwidth 128
Main(config)#int s0/0/1
Main(config-if)#bandwidth 128

Bldg-2(config)#int s0/0/1
Bldg-2(config-if)#bandwidth 128
{% endhighlight bash %}

Configure OSPF router IDs as follows:

* Building 1: 1.1.1.1

{% highlight bash %}
Bldg-1(config)#router ospf 1
Bldg-1(config-router)#router-id 1.1.1.1
{% endhighlight bash %}

* Main: 2.2.2.2

{% highlight bash %}
Main(config)#router ospf 1
Main(config-router)#router-id 2.2.2.2
{% endhighlight bash %}

* Building 2: 3.3.3.3

{% highlight bash %}
Bldg-2(config)#router ospf 1
Bldg-2(config-router)#router-id 3.3.3.3
{% endhighlight bash %}

Configure the OSPF cost of the link between Building 1 and Main to 7500.

{% highlight bash %}
Bldg-1(config)#int s0/0/0
Bldg-1(config-if)#ip ospf cost 7500

Main(config)#int s0/0/0
Main(config-if)#ip ospf cost 7500
{% endhighlight bash %}

***

## Step 8: Configure OSPF MD5 Authentication on the Required Interfaces

***

#### **Task:**
Configure OSPF to authenticate routing updates with MD5 authentication on the OSPF interfaces.

* Use a key value of 1.
* Use xyz_OSPF as the password.
* Apply MD5 authentication to the required interfaces.

#### **How:**
* Use a key value of 1.
* Use xyz_OSPF as the password.
* Apply MD5 authentication to the required interfaces.

{% highlight bash %}
Bldg-1(config)#int s0/0/0
Bldg-1(config-if)#ip ospf message-digest-key 1 md5 xyz_OSPF
Bldg-1(config-if)#ip ospf authentication message-digest

Main(config)#int s0/1/0
Main(config-if)#ip ospf message-digest-key 1 md5 xyz_OSPF
Main(config-if)#ip ospf authentication message-digest
Main(config)#int s0/0/1
Main(config-if)#ip ospf message-digest-key 1 md5 xyz_OSPF
Main(config-if)#ip ospf authentication message-digest

Bldg-2(config)#int s0/0/1
Bldg-2(config-if)#ip ospf message-digest-key 1 md5 xyz_OSPF
Bldg-2(config-if)#ip ospf authentication message-digest
{% endhighlight bash %}

***

## Step 9: Configure Access Control Lists

***

#### **Task/How:**
Restrict access to the vty lines on Main with an ACL:

* Create a named standard ACL using the name TELNET-BLOCK. Be sure that you enter this name exactly as it appears in this instruction.

{% highlight bash %}
Main(config)#ip access-list standard TELNET-BLOCK
{% endhighlight bash %}

* Allow only Admin Host to access the vty lines of Main.
* No other Internet hosts (including hosts not visible in the topology) should be able to access the vty lines of Main.
* Your solution should consist of one ACL statement.
* Your ACL should be placed in the most efficient location as possible to conserve network bandwidth and device processing resources.

{% highlight bash %}
Main(config-std-nacl)#permit host 198.51.100.5
Main(config)#line vty 0 15
Main(config-line)#access-class TELNET-BLOCK in
{% endhighlight bash %}

Block ping requests from the Internet with an ACL:

* Use access list number 101.

{% highlight bash %}
Main(config)#interface serial 0/1/0
Main(config-if)#ip access-group 101 in
{% endhighlight bash %}

* Allow only Admin Host to ping addresses within the Company A (Main) network. Only echo messages should be permitted.
* Prevent all other Internet hosts (not only the Internet hosts visible in the topology) from pinging addresses inside the Company A network. Block echo messages only.
* All other traffic should be allowed.
* Your ACL should consist of three statements.
* Your ACL should be placed in the most efficient location as possible to conserve network bandwidth and device processing resources.

Control access to the management interfaces (SVI) of the three switches attached to Building 2 as follows:

* Create a standard ACL.
* Use the number 1 for the list.

{% highlight bash %}
Bldg-2(config)#access-list 1 permit 10.10.15.0 0.0.0.255
{% endhighlight bash %}

* Permit only addresses from the admin VLAN network to access any address on the manage network (VLAN25).

{% highlight bash %}
Bldg-2(config)#interface gi0/1.25
Bldg-2(config-if)#ip access-group 1 out
{% endhighlight bash %}

* Hosts on the NetAdmin VLAN network should be able to reach all other destinations.
* Your list should consist of one statement.
* Your ACL should be placed in the most efficient location as possible to conserve network bandwidth and device processing resources.
* You will be able to test this ACL at the end of this assessment.

***

## Step 10: Create and name VLANs

***

#### **Task:**
On all three switches that are attached to Building 2, create and name the VLANs shown in the VLAN Table.

* The VLAN names that you configure must match the values in the table exactly.
* Each switch should be configured with all of the VLANs shown in the table.

#### **How:**
* The VLAN names that you configure must match the values in the table exactly.
* Each switch should be configured with all of the VLANs shown in the table.

{% highlight bash %}
FL-A(config)#vlan 2
FL-A(config-vlan)#name dept1
FL-A(config)#vlan 4
FL-A(config-vlan)#name dept2
FL-A(config)#vlan 8
FL-A(config-vlan)#name dept3
FL-A(config)#vlan 15
FL-A(config-vlan)#name NetAdmin
FL-A(config)#vlan 25
FL-A(config-vlan)#name manage
FL-A(config)#vlan 99
FL-A(config-vlan)#name safe

FL-B(config)#vlan 2
FL-B(config-vlan)#name dept1
FL-B(config)#vlan 4
FL-B(config-vlan)#name dept2
FL-B(config)#vlan 8
FL-B(config-vlan)#name dept3
FL-B(config)#vlan 15
FL-B(config-vlan)#name NetAdmin
FL-B(config)#vlan 25
FL-B(config-vlan)#name manage
FL-B(config)#vlan 99
FL-B(config-vlan)#name safe

FL-C(config)#vlan 2
FL-C(config-vlan)#name dept1
FL-C(config)#vlan 4
FL-C(config-vlan)#name dept2
FL-C(config)#vlan 8
FL-C(config-vlan)#name dept3
FL-C(config)#vlan 15
FL-C(config-vlan)#name NetAdmin
FL-C(config)#vlan 25
FL-C(config-vlan)#name manage
FL-C(config)#vlan 99
FL-C(config-vlan)#name safe
{% endhighlight bash %}

***

## Step 11:  Assign switch ports to VLANs.

***

#### **Task:**
Using the VLAN table, assign switch ports to the VLANs you created in Step 10, as follows:

* All switch ports that you assign to VLANs should be configured to static access mode.
* All switch ports that you assign to VLANs should be activated.
* Note that all of the unused ports on FL-A should be assigned to VLAN 99. This configuration step on switches FL-B and FL-C is not required in this assessment for the sake of time.
* Secure the unused switch ports on FL-A by shutting them down.

#### **How:**
Using the VLAN table, assign switch ports to the VLANs you created in Step 10, as follows:

* All switch ports that you assign to VLANs should be configured to static access mode.
* All switch ports that you assign to VLANs should be activated.

{% highlight bash %}
FL-A(config)#interface fa0/5
FL-A(config-if)#switchport mode access
FL-A(config-if)#switchport access vlan 2
FL-A(config-if)#no shutdown

FL-A(config)#interface fa0/10
FL-A(config-if)#switchport mode access
FL-A(config-if)#switchport access vlan 4
FL-A(config-if)#no shutdown

FL-A(config)#interface fa0/15
FL-A(config-if)#switchport mode access
FL-A(config-if)#switchport access vlan 8
FL-A(config-if)#no shutdown

FL-A(config)#interface fa0/24
FL-A(config-if)#switchport mode access
FL-A(config-if)#switchport access vlan 15
FL-A(config-if)#no shutdown

FL-C(config)#interface fa0/7
FL-C(config-if)#switchport mode access
FL-C(config-if)#switchport access vlan 2
FL-C(config-if)#no shutdown

FL-C(config)#interface fa0/10
FL-C(config-if)#switchport mode access
FL-C(config-if)#switchport access vlan 4
FL-C(config-if)#no shutdown

FL-C(config)#interface fa0/15
FL-C(config-if)#switchport mode access
FL-C(config-if)#switchport access vlan 8
FL-C(config-if)#no shutdown

FL-C(config)#interface fa0/24
FL-C(config-if)#switchport mode access
FL-C(config-if)#switchport access vlan 15
FL-C(config-if)#no shutdown
{% endhighlight bash %}

* Note that all of the unused ports on FL-A should be assigned to VLAN 99. This configuration step on switches FL-B and FL-C is not required in this assessment for the sake of time.
* Secure the unused switch ports on FL-A by shutting them down.

{% highlight bash %}
FL-A(config)#interface range fa0/6-9,fa0/11-14,fa0/16-23
FL-A(config-if-range)#switchport mode access
FL-A(config-if-range)#switchport access vlan 99
FL-A(config-if-range)#shutdown

FL-A(config)#interface range g0/1-2
FL-A(config-if-range)#switchport mode access
FL-A(config-if-range)#switchport access vlan 99
FL-A(config-if-range)#shutdown
{% endhighlight bash %}

***

## Step 12:  Configure the SVIs

***

#### **Task:**
Refer to the Addressing Table. Create and address the SVIs on all three of the switches that are attached to Building 2. Configure the switches so that they can communicate with hosts on other networks. Full connectivity will be established after routing between VLANs has been configured later in this assessment.

#### **How:**
{% highlight bash %}
FL-A(config)#ip default-gateway 10.10.25.1
FL-A(config)#interface vlan 25
FL-A(config-vlan)#ip address 10.10.25.254 255.255.255.0
FL-A(config-vlan)#no shutdown

FL-B(config)#ip default-gateway 10.10.25.1
FL-B(config)#interface vlan 25
FL-B(config-vlan)#ip address 10.10.25.253 255.255.255.0
FL-B(config-vlan)#no shutdown

FL-C(config)#ip default-gateway 10.10.25.1
FL-C(config)#interface vlan 25
FL-C(config-vlan)#ip address 10.10.25.252 255.255.255.0
FL-C(config-vlan)#no shutdown
{% endhighlight bash %}

***

## Step 13: Configure Trunking and EtherChannel

***

#### **Task/How:**
Use the information in the Port-Channel Groups table to configure EtherChannel as follows:

* Use LACP.
* The switch ports on both sides of Channels 1 and 2 should initiate negotiations for channel establishment.

{% highlight bash %}
FL-A(config)#interface range fa0/1-2
FL-A(config-if-range)#channel-group 1 mode active
FL-A(config-if-range)#no shutdown

FL-A(config)#interface range fa0/3-4
FL-A(config-if-range)#channel-group 2 mode active
FL-A(config-if-range)#no shutdown

FL-A(config)#interface port-channel 1
FL-A(config-if)#switchport mode trunk

FL-A(config)#interface port-channel 2
FL-A(config-if)#switchport mode trunk

FL-B(config)#interface range fa0/3-4
FL-B(config-if-range)#channel-group 2 mode active
FL-B(config-if-range)#no shutdown

FL-B(config)#interface port-channel 2
FL-B(config-if)#switchport mode trunk

FL-C(config)#interface range fa0/1-2
FL-C(config-if-range)#channel-group 1 mode active
FL-C(config-if-range)#no shutdown

FL-C(config)#interface port-channel 1
FL-C(config-if)#switchport mode trunk
{% endhighlight bash %}

* The switch ports on the FL-B side of Channel 3 should initiate negotiations with the switch ports on FL-C.

{% highlight bash %}
FL-B(config)#interface range fa0/5-6
FL-B(config-if-range)#channel-group 3 mode active
FL-B(config-if-range)#no shutdown

FL-B(config)#interface port-channel 3
FL-B(config-if)#switchport mode trunk
{% endhighlight bash %}

* The switch ports on the FL-C side of Channel 3 should not initiate negotiations with the switch ports on the other side of the channel.

{% highlight bash %}
FL-C(config)#interface range fa0/5-6
FL-C(config-if-range)#channel-group 3 mode passive
FL-B(config-if-range)#no shutdown

FL-C(config)#interface port-channel 3
FL-C(config-if)#switchport mode trunk
{% endhighlight bash %}

* All channels should be ready to forward data after they have been configured.

{% highlight bash %}
FL-X(config-if)#no shutdown (if you didn't do it prior)
{% endhighlight bash %}

Configure all port-channel interfaces as trunks.

{% highlight bash %}
FL-X(config)#interface port-channel X (again you should have done it prior)
FL-X(config-if)#switchport mode trunk
{% endhighlight bash %}

Configure static trunking on the switch port on FL-B that is connected to Building 2.

{% highlight bash %}
FL-B(config)#interface g0/1
FL-B(config-if)#switchport mode trunk
{% endhighlight bash %}

***

## Step 14: Configure Rapid PVST+

***

#### **Task/How:**
Configure Rapid PVST+ as follows:

1. Activate Rapid PVST+ and set root priorities.

* All three switches should be configured to run Rapid PVST+.

{% highlight bash %}
FL-A(config)#spanning-tree mode rapid-pvst
FL-B(config)#spanning-tree mode rapid-pvst
FL-C(config)#spanning-tree mode rapid-pvst
{% endhighlight bash %}

* FL-A should be configured as root primary for VLAN 2 and VLAN 4 using the default primary priority values.

{% highlight bash %}
FL-A(config)#spanning-tree vlan 2 root primary
FL-A(config)#spanning-tree vlan 4 root primary
{% endhighlight bash %}

* FL-A should be configured as root secondary for VLAN 8 and VLAN 15 using the default secondary priority values.

{% highlight bash %}
FL-A(config)#spanning-tree vlan 8 root secondary
FL-A(config)#spanning-tree vlan 15 root secondary
{% endhighlight bash %}

* FL-C should be configured as root primary for VLAN 8 and VLAN 15 using the default primary priority values.

{% highlight bash %}
FL-C(config)#spanning-tree vlan 8 root primary
FL-C(config)#spanning-tree vlan 15 root primary
{% endhighlight bash %}

* FL-C should be configured as root secondary for VLAN 2 and VLAN 4 using the default secondary priority values.

{% highlight bash %}
FL-C(config)#spanning-tree vlan 2 root secondary
FL-C(config)#spanning-tree vlan 4 root secondary
{% endhighlight bash %}

2. Activate PortFast and BPDU Guard on the active FL-C switch access ports.

* On FL-C, configure PortFast on the access ports that are connected to hosts.

{% highlight bash %}
FL-C(config)#interface range fa0/7, fa0/10, fa0/15, fa0/24
FL-C(config-if-range)#spanning-tree portfast
{% endhighlight bash %}

* On FL-C, activate BPDU Guard on the access ports that are connected to hosts.

{% highlight bash %}
FL-C(config)#interface range fa0/7, fa0/10, fa0/15, fa0/24
FL-C(config-if-range)#spanning-tree bpduguard enable
FL-C(config-if-range)#no shutdown
{% endhighlight bash %}

***

## Step 15: Configure switch security

***

#### **Task/How:**
You are required to complete the following only on some of the devices in the network for this assessment. In reality, security should be configured on all devices in the network.

Configure port security on all active access ports that have hosts connected on FL-A.

{% highlight bash %}
FL-A(config)#interface fa0/5
FL-A(config-if)#switchport port-security

FL-A(config)#interface fa0/10
FL-A(config-if)#switchport port-security

FL-A(config)#interface fa0/15
FL-A(config-if)#switchport port-security

FL-A(config)#interface fa0/24
FL-A(config-if)#switchport port-security
{% endhighlight bash %}

* Each active access port should accept only two MAC addresses before a security action occurs.

{% highlight bash %}
FL-A(config)#interface fa0/5
FL-A(config-if)#switchport port-security maximum 2
FL-A(config)#interface fa0/10
FL-A(config-if)#switchport port-security maximum 2
FL-A(config)#interface fa0/15
FL-A(config-if)#switchport port-security maximum 2
FL-A(config)#interface fa0/24
FL-A(config-if)#switchport port-security maximum 2
{% endhighlight bash %}

* The learned MAC addresses should be recorded in the running configuration.

{% highlight bash %}
FL-A(config)#interface fa0/5
FL-A(config-if)#switchport port-security mac-address sticky
FL-A(config)#interface fa0/10
FL-A(config-if)#switchport port-security mac-address sticky
FL-A(config)#interface fa0/15
FL-A(config-if)#switchport port-security mac-address sticky
FL-A(config)#interface fa0/24
FL-A(config-if)#switchport port-security mac-address sticky
{% endhighlight bash %}

* If a security violation occurs, the switch ports should provide notification that a violation has occurred but not place the interface in an err-disabled state.

{% highlight bash %}
FL-A(config)#interface fa0/5
FL-A(config-if)#switchport port-security violation restrict
FL-A(config)#interface fa0/10
FL-A(config-if)#switchport port-security violation restrict
FL-A(config)#interface fa0/15
FL-A(config-if)#switchport port-security violation restrict
FL-A(config)#interface fa0/24
FL-A(config-if)#switchport port-security violation restrict
{% endhighlight bash %}

On FL-B, configure the virtual terminal lines to accept only SSH connections.

* Use a domain name of ccnaPTSA.com.

{% highlight bash %}
FL-B(config)#ip domain-name ccnaPTSA.com
{% endhighlight bash %}

* Use FL-B as the host name.

{% highlight bash %}
FL-B(config)#hostname FL-B
{% endhighlight bash %}

* Use a modulus value of 1024.

{% highlight bash %}
FL-B(config)#crypto key generate rsa
select 1024 key length
{% endhighlight bash %}

* Configure SSH version 2.

{% highlight bash %}
FL-B(config)#ip ssh version 2
{% endhighlight bash %}

* Configure the vty lines to only accept SSH connections.

{% highlight bash %}
FL-B(config)#line vty 0 4
FL-B(config-line)#login local
FL-B(config-line)#transport input ssh
FL-B(config)#line vty 5 15
FL-B(config-line)#login local
FL-B(config-line)#transport input ssh
{% endhighlight bash %}

* Configure user-based authentication for the SSH connections with a user name of netadmin and a secret password of SSH_secret9. The user name and password must match the values provided here exactly.

{% highlight bash %}
FL-B(config)#username netadmin password SSH_secret9
{% endhighlight bash %}

Ensure that all unused switch ports on FL-A have been secured as follows:

* They should be assigned to VLAN 99.
* They should all be in access mode.
* They should be shutdown.

{% highlight bash %}
Should have been done in Step 11, but here they are anyway

FL-A(config)#interface range fa0/6-9,fa0/11-14,fa0/16-23
FL-A(config-if-range)#switchport mode access
FL-A(config-if-range)#switchport access vlan 99
FL-A(config-if-range)#shutdown

FL-A(config)#interface range g0/1-2
FL-A(config-if-range)#switchport mode access
FL-A(config-if-range)#switchport access vlan 99
FL-A(config-if-range)#shutdown
{% endhighlight bash %}

***

## Step 16: Configure Building 2 as a DHCP server for the hosts attached to the FL-A and FL-C switches

***

#### **Task/How:**
Configure three DHCP pools as follows:

* Refer to the information in the Addressing Table.
* Create a DHCP pool for hosts on VLAN 2 using the pool name vlan2pool.

{% highlight bash %}
Bldg-2(config)#ip dhcp pool vlan2pool
Bldg-2(dhcp-config)#network 10.10.2.0 255.255.255.0
Bldg-2(dhcp-config)#default-router 10.10.2.1
Bldg-2(dhcp-config)#dns-server 192.168.200.225
{% endhighlight bash %}

* Create a DHCP pool for hosts on VLAN 4 using the pool name vlan4pool.

{% highlight bash %}
Bldg-2(config)#ip dhcp pool vlan4pool
Bldg-2(dhcp-config)#network 10.10.4.0 255.255.255.0
Bldg-2(dhcp-config)#default-router 10.10.4.1
Bldg-2(dhcp-config)#dns-server 192.168.200.225
{% endhighlight bash %}

* Create a DHCP pool for hosts on VLAN 8 using the pool name vlan8pool.

{% highlight bash %}
Bldg-2(config)#ip dhcp pool vlan8pool
Bldg-2(dhcp-config)#network 10.10.8.0 255.255.255.0
Bldg-2(dhcp-config)#default-router 10.10.8.1
Bldg-2(dhcp-config)#dns-server 192.168.200.225
{% endhighlight bash %}

* All VLAN pool names must match the provided values exactly.
* Exclude the first five addresses from each pool.

{% highlight bash %}
Bldg-2(config)#ip dhcp excluded-address 10.10.2.1 10.10.2.5
Bldg-2(config)#ip dhcp excluded-address 10.10.4.1 10.10.4.5
Bldg-2(config)#ip dhcp excluded-address 10.10.8.1 10.10.8.5
{% endhighlight bash %}

* Configure a DNS server address of 192.168.200.225.

{% highlight bash %}
Bldg-2(dhcp-config)#dns-server 192.168.200.225 (this command from above)
{% endhighlight bash %}

* All hosts should be able to communication with hosts on other networks.

{% highlight bash %}
Bldg-2(dhcp-config)#default-router 10.10.X.1 (this command from above)
{% endhighlight bash %}

***

## Step 17: Configure host addressing

***

#### **Task:**
Hosts should be able to ping each other and external hosts after they have been correctly addressed, where permitted.

* Hosts on VLANs 2, 4, and 8 should be configured to receive addresses dynamically over DHCP.
* Hosts on VLAN 15 should be addressed statically as indicated in the Addressing Table. Once configured, the hosts should be able to ping hosts on other networks.
* Hosts on the LANs attached to Site 1 should be statically assigned addresses that enable them to communicate with hosts on other networks, as indicated in the Addressing Table.

#### **How:**
Simply setup all the addressing manually through the GUI on each workstation from the IP Addressing table
