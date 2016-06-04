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
 - frame-relay
 - gre
---

## <center>Contents</center>

---

1. **Initialize Devices**
  * 1.1. Initialize and reload routers
2. **Configure Device Basic Settings**
  * 2.1. Configure PCs
  * 2.2. Configure R1
  * 2.3. Configure R2
  * 2.4. Configure R3
  * 2.5. Save device configurations to Flash
3. **Configure PPP Connections**
  * 3.1. Configure R1
  * 3.2. Configure R2
  * 3.3. Configure R3
  * 3.4. Verify network connectivity
4. **Configure NAT**
  * 4.1. Configure R2
  * 4.2. Verify network connectivity
  * 4.3. Verify NAT Configuration on R2
5. **Monitor the Network**
  * 5.1. Configure NTP
  * 5.2. Configure Syslog messaging
  * 5.3. Configure SNMP on R1
  * 5.4. Collect NetFlow data on R2
  * 5.5. Verify monitoring configurations
6. **Configure Frame Relay**
  * 6.1. Reload routers and restore the BasicConfig to memory
  * 6.2. Configure R2 as a Frame Relay Switch
  * 6.3. Configure R1
  * 6.4. Configure R3
  * 6.5. Verify network connectivity
  * 6.6. Verify Frame Relay configuration
7. **Configure a GRE VPN Tunnel**
  * 7.1. Reload routers and restore the BasicConfig to memory
  * 7.2. Configure Serial Interfaces
  * 7.3. Configure the GRE VPN tunnel and EIGRP on R1
  * 7.4. Configure the GRE VPN tunnel and EIGRP on R3
  * 7.5. Verify network connectivity
  * 7.6. Verify GRE VPN configuration

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

![PC-A]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-pc-a.png){:.center-image}

Configure static IPv4 address information on PC-B.

![PC-B]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-pc-b.png){:.center-image}

Configure static IPv4 address information on PC-C.

![PC-C]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-pc-c.png){:.center-image}

#### **Configure R1**

Disable DNS lookup

{% highlight bash %}
Router(config)#no ip domain-lookup
{% endhighlight bash %}

Router name

{% highlight bash %}
Router(config)#hostname R1
{% endhighlight bash %}

Encrypted privileged EXEC password

{% highlight bash %}
R1(config)#enable secret class
{% endhighlight bash %}

Console access password

{% highlight bash %}
R1(config)#line console 0
R1(config-line)#password cisco
R1(config-line)#login
{% endhighlight bash %}

Telnet access password

{% highlight bash %}
R1(config)#line vty 0 15
R1(config-line)#password cisco
R1(config-line)#login
{% endhighlight bash %}

Encrypt the plain text passwords

{% highlight bash %}
R1(config)#service password-encryption
{% endhighlight bash %}

MOTD banner

{% highlight bash %}
R1(config)#banner motd &Unauthorized Access is Prohibited!&
{% endhighlight bash %}

Configure G0/0

{% highlight bash %}
R1(config)#int g0/0
R1(config-if)#description Connection to 192.168.11.0 LAN
R1(config-if)#ip address 192.168.11.1 255.255.255.0
R1(config-if)#no shutdown

%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
{% endhighlight bash %}

#### **Configure R2**

Disable DNS lookup

{% highlight bash %}
Router(config)#no ip domain-lookup
{% endhighlight bash %}

Router name

{% highlight bash %}
Router(config)#hostname R2
{% endhighlight bash %}

Encrypted privileged EXEC password

{% highlight bash %}
R2(config)#enable secret class
{% endhighlight bash %}

Console access password

{% highlight bash %}
R2(config)#line console 0
R2(config-line)#password cisco
R2(config-line)#login
{% endhighlight bash %}

Telnet access password

{% highlight bash %}
R2(config)#line vty 0 15
R2(config-line)#password cisco
R2(config-line)#login
{% endhighlight bash %}

Encrypt the plain text passwords

{% highlight bash %}
R2(config)#service password-encryption
{% endhighlight bash %}

MOTD banner

{% highlight bash %}
R2(config)#banner motd &Unauthorized Access is Prohibited!&
{% endhighlight bash %}

Configure G0/0

{% highlight bash %}
R2(config)#int g0/0
R2(config-if)#description Connection to 192.168.22.0 LAN
R2(config-if)#ip address 192.168.22.1 255.255.255.0
R2(config-if)#no shutdown

%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
{% endhighlight bash %}

#### **Configure R3**

Disable DNS lookup

{% highlight bash %}
Router(config)#no ip domain-lookup
{% endhighlight bash %}

Router name

{% highlight bash %}
Router(config)#hostname R3
{% endhighlight bash %}

Encrypted privileged EXEC password

{% highlight bash %}
R3(config)#enable secret class
{% endhighlight bash %}

Console access password

{% highlight bash %}
R3(config)#line console 0
R3(config-line)#password cisco
R3(config-line)#login
{% endhighlight bash %}

Telnet access password

{% highlight bash %}
R3(config)#line vty 0 15
R3(config-line)#password cisco
R3(config-line)#login
{% endhighlight bash %}

Encrypt the plain text passwords

{% highlight bash %}
R3(config)#service password-encryption
{% endhighlight bash %}

MOTD banner

{% highlight bash %}
R3(config)#banner motd &Unauthorized Access is Prohibited!&
{% endhighlight bash %}

Configure G0/0

{% highlight bash %}
R3(config)#int g0/0
R3(config-if)#description Connection to 192.168.33.0 LAN
R3(config-if)#ip address 192.168.33.1 255.255.255.0
R3(config-if)#no shutdown

%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
{% endhighlight bash %}

#### **Save device configurations to Flash**

Copy the running-config on R1 to flash. Name the file **BasicConfig**

{% highlight bash %}
R1#copy running-config BasicConfig
R1#copy running-config startup-config 
{% endhighlight bash %}

Copy the running-config on R2 to flash. Name the file **BasicConfig**

{% highlight bash %}
R2#copy running-config BasicConfig
R2#copy running-config startup-config 
{% endhighlight bash %}

Copy the running-config on R3 to flash. Name the file **BasicConfig**

{% highlight bash %}
R3#copy running-config BasicConfig
R3#copy running-config startup-config 
{% endhighlight bash %}

---

## <center>3. Configure PPP Connections</center>

---

#### **Configure R1**

Configure S0/0/0

{% highlight bash %}
R1(config)#int s0/0/0
R1(config-if)#description PPP Connection to R2
R1(config-if)#ip address 172.27.12.1 255.255.255.252
R1(config-if)#clock rate 128000
R1(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to down
{% endhighlight bash %}

Configure CHAP authentication on S0/0/0

{% highlight bash %}
R1(config-if)#encapsulation ppp
R1(config-if)#ppp authentication chap
{% endhighlight bash %}

Create a local database entry for CHAP authentication

{% highlight bash %}
R1(config)#username R2 password cisco
{% endhighlight bash %}

Set a static default route out S0/0/0

{% highlight bash %}
R1(config)#ip route 0.0.0.0 0.0.0.0 s0/0/0

%Default route without gateway, if not a point-to-point interface, may impact performance
{% endhighlight bash %}

#### **Configure R2**

Configure S0/0/0

{% highlight bash %}
R2(config)#int s0/0/0
R2(config-if)#description PPP Connection to R1
R2(config-if)#ip address 172.27.12.2 255.255.255.252
R2(config-if)#no shutdown
{% endhighlight bash %}

Configure CHAP authentication on S0/0/0

{% highlight bash %}
R2(config-if)#encapsulation ppp
R2(config-if)#ppp authentication chap
{% endhighlight bash %}

Create a local database entry for CHAP authentication

{% highlight bash %}
R2(config-if)#username R1 password cisco
{% endhighlight bash %}

Configure S0/0/1

{% highlight bash %}
R2(config)#int s0/0/1
R2(config-if)#description PPP Connection to ISP
R2(config-if)#ip address 209.165.200.225 255.255.255.248
R2(config-if)#clock rate 128000
R2(config-if)#no shutdown
R2(config-if)#encapsulation ppp
{% endhighlight bash %}

Set a static default route out S0/0/1

{% highlight bash %}
R2(config)#ip route 0.0.0.0 0.0.0.0 s0/0/1

%Default route without gateway, if not a point-to-point interface, may impact performance
{% endhighlight bash %}

Set a static route for R1 LAN traffic out S0/0/0

{% highlight bash %}
R2(config)#ip route 192.168.11.0 255.255.255.0 s0/0/0

%Default route without gateway, if not a point-to-point interface, may impact performance
{% endhighlight bash %}

#### **Configure R3**

Configure S0/0/1

{% highlight bash %}
R3(config)#int s0/0/1
R3(config-if)#description PPP Connection R2
R3(config-if)#ip address 209.165.200.230 255.255.255.248
R3(config-if)#encapsulation ppp
R3(config-if)#no shutdown
{% endhighlight bash %}

#### **Verify network connectivity**

![Ping Test]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-ping-test.png){:.center-image}

---

## <center>4. Configure NAT</center>

---

#### **Configure R2**

Assign a static NAT to map the inside local IP address for PC-B to a Inside Global address

{% highlight bash %}
R2(config)#ip nat inside source static 192.168.22.3 209.165.200.226
{% endhighlight bash %}

Define an access control list to permit the R1 LAN for dynamic NAT

{% highlight bash %}
R2(config)#access-list 1 permit 192.168.11.0 0.0.0.255
{% endhighlight bash %}

Define the dynamic NAT pool for the R1 LAN

{% highlight bash %}
R2(config)#ip nat pool R1-LAN 209.165.200.227 209.165.200.227 netmask 255.255.255.248
{% endhighlight bash %}

Define the NAT from the inside source to the outside pool. Make sure to allow multiple PCs access to this single Inside Global address

{% highlight bash %}
R2(config)#ip nat inside source list 1 pool R1-LAN overload
R2(config)#ip nat pool R1-LAN 209.165.200.227 209.165.200.227 netmask
{% endhighlight bash %}

Define an access control list to permit the R2 LAN for dynamic NAT

{% highlight bash %}
R2(config)#access-list 2 permit 192.168.22.0 0.0.0.255
{% endhighlight bash %}

Define the dynamic NAT pool for the R2 LAN

{% highlight bash %}
R2(config)#ip nat pool R2-LAN 209.165.200.228 209.165.200.228 netmask 255.255.255.248
{% endhighlight bash %}

Define the NAT from the inside source to the outside pool. Make sure to allow multiple PCs access to this single Inside Global address

{% highlight bash %}
R2(config)#ip nat pool R2-LAN 209.165.200.228 209.165.200.228 netmask
R2(config)#ip nat inside source list 2 pool R2-LAN overload
{% endhighlight bash %}

Assign the outside NAT interface

{% highlight bash %}
R2(config)#int s0/0/1
R2(config-f)#ip nat outside
{% endhighlight bash %}

Assign the inside NAT interface for the R1 LAN

{% highlight bash %}
R2(config)#int s0/0/0
R2(config-f)#ip nat inside
{% endhighlight bash %}

Assign the inside NAT interface for the R2 LAN

{% highlight bash %}
R2(config)#int g0/0
R2(config-f)#ip nat inside
{% endhighlight bash %}

#### **Verify network connectivity**

![Ping Test 2]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-ping-test-2.png){:.center-image}

#### **Verify NAT Configuration on R2**

Display configured access lists

{% highlight bash %}
R2#show access-lists
{% endhighlight bash %}

Display the current active NAT translations

{% highlight bash %}
R2#show ip nat translations
{% endhighlight bash %}

Display detailed information about NAT including interface, access list, and pool assignments

{% highlight bash %}
R2#show ip nat statistics
{% endhighlight bash %}

---

## <center>5. Monitor the Network</center>

---

#### **Configure NTP**

Set the clock on R2 to a date and time specified for NTP testing

{% highlight bash %}
R2#clock set 9:00:00 25 August 2013
{% endhighlight bash %}

Configure R2 as the NTP Master

{% highlight bash %}
R2(config)#ntp master 5
{% endhighlight bash %}

Configure R1 so that it uses R2 as its NTP Server

{% highlight bash %}
R1(config)#ntp server 172.27.12.2
{% endhighlight bash %}

#### **Configure Syslog messaging**

Enable the timestamp service on R1 and R2 for system logging purposes

{% highlight bash %}
R1(config)#services timestamp log datetime msec
R2(config)#services timestamp log datetime msec
{% endhighlight bash %}

Enable logging of messages on R1 and R2

{% highlight bash %}
R1(config)#logging host 192.168.11.3
R2(config)#logging host 192.168.11.3
{% endhighlight bash %}

Change message trapping level on R1 and R2

{% highlight bash %}
R1(config)#logging trap debugging
R2(config)#logging trap debugging
{% endhighlight bash %}

#### **Configure SNMP on R1**

Create a standard access list to permit the SNMP management station (PC-A) to retrieve SNMP information from R1

{% highlight bash %}
R1(config)#ip access-list standard SNMP-ACCESS
R1(config-std-nacl)#permit 192.168.11.3
{% endhighlight bash %}

Enable SNMP community access to the SNMP-ACCESS access list

{% highlight bash %}
**HELP**
R1(config)#snmp-server community SA-LAB ro SNMP-ACCESS
{% endhighlight bash %}

Set the SNMP notification host

{% highlight bash %}
**HELP**
R1(config)#snmp-server host 192.168.11.3 version 2c SA-LAB
{% endhighlight bash %}

Enable all SNMP traps

{% highlight bash %}
**HELP**
R1(config)#snmp-server enable traps
{% endhighlight bash %}

#### **Collect NetFlow data on R2**

Configure NetFlow data capture on both serial interfaces. Capture ingress and egress data packets

{% highlight bash %}
R2(config)#int s0/0/0
R2(config-if)#ip flow ingress
R2(config-if)#ip flow egress
R2(config)#int s0/0/1
R2(config-if)#ip flow ingress
R2(config-if)#ip flow egress
{% endhighlight bash %}

Configure NetFlow data export

{% highlight bash %}
R2(config)#ip flow-export destination 192.168.22.3 9996
{% endhighlight bash %}

Configure the NetFlow export version

{% highlight bash %}
R2(config)#ip flow-export version 9
{% endhighlight bash %}

#### **Verify monitoring configurations**

Display the date and time

{% highlight bash %}
R2#show clock
{% endhighlight bash %}

Display the contents of logging buffers

{% highlight bash %}
R2#show logging
{% endhighlight bash %}

Display information about the SNMP communities

{% highlight bash %}
R2#show snmp community
{% endhighlight bash %}

Display the protocol using the highest volume of traffic

{% highlight bash %}
R2#show ip cache flow
{% endhighlight bash %}

---

## <center>6. Configure Frame Relay</center>

---

![Frame Relay Topology]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-frame-topology.png){:.center-image}

#### **Reload routers and restore the BasicConfig to memory**

Erase the startup configurations and reload the devices

{% highlight bash %}
R1#show flash
R1#reload
R2#show flash
R2#reload
R3#show flash
R3#reload
{% endhighlight bash %}

For each router, issue the **copy flash:BasicConfig running-config** command to reload the basic configuration that you saved at the end of Part 2

{% highlight bash %}
R1#copy flash:BasicConfig running-config
R2#copy flash:BasicConfig running-config
R3#copy flash:BasicConfig running-config
{% endhighlight bash %}

Issue the **no shutdown** command for the G0/0 interface on R1 and R3

{% highlight bash %}
R1(config)#int g0/0
R1(config-if)#no shutdown
R3(config)#int g0/0
R3(config-if)#no shutdown
{% endhighlight bash %}

#### **Configure R2 as a Frame Relay Switch**

{% highlight bash %}
R2(config)#frame-relay switching
R2(config)#int s0/0/0
R2(config-if)#encapsulation frame-relay
R2(config-if)#frame-relay intf-type dce
R2(config-if)#frame-relay route 123 interface s0/0/1 321
R2(config-if)#frame-relay lmi-type ansi
R2(config-if)#no shutdown
R2(config-if)#int s0/0/1
R2(config-if)#clock rate 128000
R2(config-if)#encapsulation frame-relay ietf
R2(config-if)#frame-relay intf-type dce
R2(config-if)#frame-relay route 321 interface s0/0/0 123
R2(config-if)#no shutdown
{% endhighlight bash %}

#### **Configure R1**

Configure S0/0/0

{% highlight bash %}
R1(config)#int s0/0/0
R1(config-if)#description Frame-Relay Connection to R3
R1(config-if)#ip address 172.27.13.1 255.255.255.252
R1(config-if)#encapsulation frame-relay
R1(config-if)#clock rate 128000
R1(config-if)#no shutdown
{% endhighlight bash %}

Disable Inverse ARP on S0/0/0

{% highlight bash %}
R1(config)#int s0/0/0
R1(config-if)#no frame-relay inverse-arp
{% endhighlight bash %}

Map the IP local address to the DLCI

{% highlight bash %}
R1(config-if)#frame-relay map ip 172.27.13.1 123
{% endhighlight bash %}

Map the remote IP address to the DLCI. Allow for multicast or broadcast traffic

{% highlight bash %}
R1(config-if)#frame-relay map ip 172.27.13.2 123 broadcast
{% endhighlight bash %}

Change the LMI type to the ANSI standard

{% highlight bash %}
R1(config-if)#frame-relay lmi-type ansi
{% endhighlight bash %}

Activate the interface

{% highlight bash %}
R1(config-if)#no shutdown
{% endhighlight bash %}

Create a default route to the IP address on the other side of the Frame Relay link

{% highlight bash %}
R1(config)#ip route 0.0.0.0 0.0.0.0 172.27.13.2
{% endhighlight bash %}

#### **Configure R3**

Configure S0/0/1

{% highlight bash %}
R3(config)#int s0/0/1
R3(config-if)#description Frame Relay Connection to R1
R3(config-if)#encapsulation frame-relay ietf
R3(config-if)#no shutdown
{% endhighlight bash %}

Create a point-to-point subinterface on S0/0/1

{% highlight bash %}
R3(config)#int s0/0/1.321 point-to-point
R3(config-subif)#description Frame Relay Connection to R1
{% endhighlight bash %}

Set the Layer 3 IPv4 address on the subinterface

{% highlight bash %}
R3(config-subif)#ip address 172.27.13.2 255.255.255.252
{% endhighlight bash %}

Disable Inverse ARP on the subinterface

{% highlight bash %}
R3(config-subif)#no frame-relay inverse-arp
{% endhighlight bash %}

Map the subinterface to the DLCI

{% highlight bash %}
R3(config-subif)#frame-relay interface-dlci 321
{% endhighlight bash %}

Create a default route to the IP address on the other side of the Frame Relay link

{% highlight bash %}
R3(config)#ip route 0.0.0.0 0.0.0.0 172.27.13.1
{% endhighlight bash %}

#### **Verify network connectivity**

![Ping Test 3]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-ping-test-3.png){:.center-image}

#### **Verify Frame Relay configuration**

Display Frame Relay LMI statistics

{% highlight bash %}
R1#show frame-relay lmi
{% endhighlight bash %}

Display the input and output packet count totals on a Frame Relay permanent virtual circuit (PVC)

{% highlight bash %}
R1#show frame-relay pvc
{% endhighlight bash %}

Display the Frame Relay maps between DLCIs and IP addresses

{% highlight bash %}
R1#show frame-relay map
{% endhighlight bash %}

---

## <center>7. Configure a GRE VPN Tunnel</center>

---

![Frame Relay Topology]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-gre-topology.png){:.center-image}

#### **Reload routers and restore the BasicConfig to memory**

Erase the startup configurations and reload the devices

{% highlight bash %}
R1#show flash
R1#reload
R2#show flash
R2#reload
R3#show flash
R3#reload
{% endhighlight bash %}

For each router, issue the **copy flash:BasicConfig running-config** command to reload the basic configuration that you saved at the end of Part 2

{% highlight bash %}
R1#copy flash:BasicConfig running-config
R2#copy flash:BasicConfig running-config
R3#copy flash:BasicConfig running-config
{% endhighlight bash %}

Issue the **no shutdown** command for the G0/0 interface on R1 and R3

{% highlight bash %}
R1(config)#int g0/0
R1(config-if)#no shutdown
R3(config)#int g0/0
R3(config-if)#no shutdown
{% endhighlight bash %}

#### **Configure Serial Interfaces**

R1 Configure S0/0/0

{% highlight bash %}
R1(config)#int s0/0/0
R1(config-if)#description HDLC Connection to ISP
R1(config-if)#ip address 172.27.12.1 255.255.255.252
R1(config-if)#no shutdown
R1(config-if)#encapsulation hdlc
R1(config-if)#clock rate 128000
{% endhighlight bash %}

R2 Configure S0/0/0

{% highlight bash %}
R2(config)#int s0/0/0
R2(config-if)#description HDLC Connection to R1
R2(config-if)#ip address 172.27.12.2 255.255.255.252
R2(config-if)#encapsulation hdlc
R2(config-if)#no shutdown
{% endhighlight bash %}

R2 Configure S0/0/1

{% highlight bash %}
R2(config)#int s0/0/1
R2(config-if)#description HDLC Connection to R3
R2(config-if)#ip address 172.27.23.2 255.255.255.252
R2(config-if)#encapsulation hdlc
R2(config-if)#clock rate 128000
R2(config-if)#no shutdown
{% endhighlight bash %}

R3 Configure S0/0/1

{% highlight bash %}
R3(config)#int s0/0/1
R3(config-if)#description HDLC Connection to ISP
R3(config-if)#ip address 172.27.23.1 255.255.255.252
R3(config-if)#encapsulation hdlc
R3(config-if)#no shutdown
{% endhighlight bash %}

#### **Configure the GRE VPN tunnel and EIGRP on R1**

Create a GRE tunnel interface

{% highlight bash %}
R1(config)#int tunnel 0
R1(config-if)#ip address 172.27.13.1 255.255.255.252
{% endhighlight bash %}

Use S0/0/0 as the tunnel source

{% highlight bash %}
R1(config-if)#tunnel source s0/0/0
{% endhighlight bash %}

Set the tunnel destination with the IP address of the R3 S0/0/1 interface

{% highlight bash %}
R1(config-if)#tunnel destination 172.27.23.1
{% endhighlight bash %}

Create a default route out S0/0/0

{% highlight bash %}
R1(config)#ip route 0.0.0.0 0.0.0.0 s0/0/0
{% endhighlight bash %}

Configure EIGRP on R1

{% highlight bash %}
R1(config)#router eigrp 1
{% endhighlight bash %}

Advertise the LAN and Tunnel subnets in EIGRP. Set the LAN interface to passive

{% highlight bash %}
R1(config-router)#network 172.27.13.0 0.0.0.3
R1(config-router)#network 192.168.11.0 0.0.0.255
R1(config-router)#passive-interface g0/0
{% endhighlight bash %}

#### **Configure the GRE VPN tunnel and EIGRP on R3**

Create a GRE tunnel interface

{% highlight bash %}
R3(config)#int tunnel 0
R3(config-if)#description GRE VPN Tunnel to R1
R3(config-if)#ip address 172.27.13.2 255.255.255.252
{% endhighlight bash %}

Use S0/0/1 as the tunnel source

{% highlight bash %}
R3(config-if)#tunnel source s0/0/1
{% endhighlight bash %}

Set the tunnel destination with the IP address of the R1 S0/0/0 interface

{% highlight bash %}
R3(config-if)#tunnel destination 172.27.12.1
{% endhighlight bash %}

Create a default route out S0/0/1

{% highlight bash %}
R3(config)#ip route 0.0.0.0 0.0.0.0 s0/0/1
{% endhighlight bash %}

Configure EIGRP on R3

{% highlight bash %}
R3(config)#router eigrp 1
{% endhighlight bash %}

Advertise the LAN and Tunnel subnets in EIGRP. Set the LAN interface to passive.

{% highlight bash %}
R3(config-router)#network 10.10.33.0 0.0.0.255
R3(config-router)#network 172.27.13.0 0.0.0.3
R3(config-router)#passive-interface g0/0
{% endhighlight bash %}

#### **Verify network connectivity**

![Ping Test 4]({{ site.url }}/images/posts/2016.06.04/ccna4-practice-skills-ping-test-4.png){:.center-image}

#### **Verify GRE VPN configuration**

Display detail information about the GRE tunnel interface

{% highlight bash %}
R1#show interface tunnel 0
{% endhighlight bash %}