---
layout: post
title: "CCNA - CLI References"
date: "2016-04-18"
comments: true
description: Some quick helpful notes for CISCO IOS devices
share: true
author: nathan
tags:
 - ccna
 - cli
---

## <center>Contents</center>

1. **Common Setup**
  * Hostname
  * Privileged Exec Password
  * Secure main Access Lines
  * Message of the Day
  * Password Length Policy
  * Password Encryption
  * Password Limit Login Attempts
  * Disable Domain Lookup
  * Save Configuration
  * Clear Configuration

2. **SSH Configure**
  * Configure Domain
  * Set Hostname on Device
  * Generate RSA Key (1024)
  * Set SSH Version 2
  * Configure vty line to accept SSH
  * Configure user-based Authentication

3. **Configure Interface**
  * Assign IP Address
  * Description
  * Speed
  * Duplex
  * Shutdown/Start up
  * DCE Clock Rate
  * Serial Bandwidth

4. **Configure Static Route**
  * Setup Default route to internet
  * Static Route to neighbor network

5. **OSPF Configuration**
  * Enable OSPF with Process ID 1
  * OSPF Router ID
  * OSPF Link Cost
  * OSPF Network assignment
  * Prevent Routing updates being sent to LAN
  * OSPF MD5 Authentication

6. **EIGRP Configuration**
  * Display the directly connected networks
  * Configure EIGRP to advertise to directly connected networks
  * Configure Passive Interface (Prevent Updates sent to LAN)
  * Disable Auto Summary
  * Verify EIGRP Routing
  * Manual Summary Calculations
  * Manual Summary Address - Example 1
  * Manual Summary Address - Example 2
  * Manual Summary of the Previous Examples
  * IPv6 Manual Summary - Example 1
  * IPv6 Manual Summary - Example 2
  * IPv6 Manual Summary of the Previous Examples

7. **VLANs and Trunking**
  * Creating VLANs
  * Assign Switch Ports to VLANs
  * Switchport Static Access
  * Sub-interface configuration
  * Defines the encapsulation format as IEEE 802.1Q (dot1q)
  * Specifies the VLAN identifier

8. **EtherChannel and Trunking**
  * Channel 1 and 2 initiate negotiations
  * Channel 3 side B should negotiate with side C
  * Channel 3 side C should not initiate negotiations with B
  * Configure Static Trunking on switchport

9. **Switch Security**
  * Configure port security on all active access ports
  * Accept only two MAC addresses
  * MAC addresses should be recorded
  * Switchport should provide notification, but not place interface in disabled state.

10. **Configure DHCP**
  * DHCP Pool Creation
  * Exclude the first five addresses from pool.

11. **Configure Access Control Lists**
  * Create a named standard ACL using the name MANAGE
  * Allow only the host on 203.0.113.18 access
  * Apply this policy to the VTY lines
  * Create an Access list with number 101
  * Allow external host 203.0.113.18 full access to inside network
  * Allow outside access to 198.51.100.14 over HTTP only
  * Allow responses to data requests to enter the Network
  * Activate access list on interface

12. **Spanning Tree Protocol**
  * Activate Rapid PVST+ and set root priorities
  * FL-A should be configured as root primary for VLAN 2 and VLAN 4 using the default primary priority values.
  * FL-A should be configured as root secondary for VLAN 8 and VLAN 15 using the default secondary priority values.
  * FL-C should be configured as root primary for VLAN 8 and VLAN 15 using the default primary priority values.
  * FL-C should be configured as root secondary for VLAN 2 and VLAN 4 using the default secondary priority values.
  * Activate PortFast and BPDU Guard on the active FL-C switch access ports.
  * On FL-C, configure PortFast on the access ports that are connected to hosts.
  * On FL-C, activate BPDU Guard on the access ports that are connected to hosts.

13. **NAT**
  * Translate the internal address of the server to the address 198.51.100.14
  * Configure the correct interfaces to perform this NAT translation
  * Configure Dynamic NAT, Use a pool name of INTERNET
  * Hosts on LAN can use Internet, source list number 1

***

#### <center>1. Common Setup</center>

***

**Configure Hostname**

{% highlight bash %}
Router(config)#hostname R1
{% endhighlight bash %}

**Privileged Exec Password**

{% highlight bash %}
R1(config)#enable secret class12345
{% endhighlight bash %}

**Secure main Access Lines**

{% highlight bash %}
R1(config)#line console 0
R1(config-line)#password cisco12345
R1(config-line)#login
R1(config-line)#logging synchronous
R1(config-line)#exec-timeout 60
R1(config-line)#exit

R1(config)#line vty 0 4
R1(config-line)#password cisco12345
R1(config-line)#transport input ssh
R1(config-line)#login local
R1(config-line)#logging synchronous
R1(config-line)#exec-timeout 60
R1(config-line)#exit

R1(config)#line aux 0
R1(config-line)#password cisco12345
R1(config-line)#login
R1(config-line)#logging synchronous
R1(config-line)#exec-timeout 60
{% endhighlight bash %}

**Message of the Day**

{% highlight bash %}
R1(config)#banner motd @Please only enter if you are cool!@
{% endhighlight bash %}

**Password Length Policy**

{% highlight bash %}
R1(config)#security passwords min-length 10
{% endhighlight bash %}

**Password Encryption**

{% highlight bash %}
R1(config)#service password-encryption
{% endhighlight bash %}

**Password Limit Login Attempts**

{% highlight bash %}
R1(config)#login block-for 120 attempts 2 within 30
{% endhighlight bash %}

**Disable Domain Lookup**

{% highlight bash %}
R1(config)#no ip domain-lookup
{% endhighlight bash %}

***

#### <center>2. SSH Configure</center>

***

**Configure Domain**

{% highlight bash %}
R1(config)#ip domain-name cisco.ua
{% endhighlight bash %}

**Set Hostname on Device**

{% highlight bash %}
R1(config)#hostname R1
{% endhighlight bash %}

**Generate RSA Key (1024)**

{% highlight bash %}
R1(config)#crypto key generate rsa
!1024
{% endhighlight bash %}

**Set SSH Version 2**

{% highlight bash %}
R1(config)#ip ssh version 2
{% endhighlight bash %}

**Configure vty line to accept SSH**

{% highlight bash %}
R1(config)#line vty 0 4
R1(config-line)#login local
R1(config-line)#transport input ssh
R1(config)#line vty 5 15
R1(config-line)#login local
R1(config-line)#transport input ssh
{% endhighlight bash %}

**Configure user-based Authentication**

{% highlight bash %}
User: netadmin
Pass: SSH_secret9
R1(config)#username netadmin password SSH_secret9
{% endhighlight bash %}

**Save Configuration**

{% highlight bash %}
R1#copy running-config startup-config
{% endhighlight bash %}

**Clear Configuration**

{% highlight bash %}
Routers:
R1# clear config all
This command will clear all configuration in NVRAM.
This command will cause ifIndex to be reassigned on the next system startup.
Do you want to continue (y/n) [n]? y

Switches:
S1# write erase
Erasing the nvram filesystem will remove all files! Continue? [confirm]y[OK]
Erase of nvram: complete
S1#
S1# reload
{% endhighlight bash %}

***

#### <center>3. Configure Interface</center>

***

**Assign IP Address**

{% highlight bash %}
R1>enable
R1#configure terminal
R1(config)#interface FastEthernet0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
{% endhighlight bash %}

**Description**

{% highlight bash %}
R1(config)#interface FastEthernet0/0
R1(config-if)#description Private LAN
{% endhighlight bash %}

**Speed**

{% highlight bash %}
R1(config)#interface FastEthernet0/0
R1(config-if)#speed 100
{% endhighlight bash %}

**Duplex**

{% highlight bash %}
R1(config)#interface FastEthernet0/0
R1(config-if)#duplex full
{% endhighlight bash %}

**Shutdown/Start up**

{% highlight bash %}
R1(config)#interface FastEthernet0/0
R1(config-if)#no shutdown

R1(config)#interface FastEthernet0/0
R1(config-if)#shutdown
{% endhighlight bash %}

**DCE Clock Rate**

{% highlight bash %}
R1(config)#int s0/0/0
R1(config-if)#clock rate 128000
{% endhighlight bash %}

**Serial Bandwidth**

{% highlight bash %}
R1(config)#int s0/0/0
R1(config-if)#bandwidth 128
{% endhighlight bash %}

***

#### <center>4. Configure Static Route</center>

***

**Setup Default route to internet**

{% highlight bash %}
R1(config)#ip route 0.0.0.0 0.0.0.0 s0/0/0
R2(config)#ip route 0.0.0.0 0.0.0.0 s0/1/0
R3(config)#ip route 0.0.0.0 0.0.0.0 s0/0/1
{% endhighlight bash %}

**Static Route to neighbour network**

{% highlight bash %}
R1(config)#ip route 192.168.200.0 255.255.252.0 s0/0/0
R2(config)#ip route 192.168.200.0 255.255.252.0 s0/0/1
{% endhighlight bash %}

***

#### <center>5. OSPF Configuration</center>

***

**<center>Network Diagram</center>**
![Addressing Table Complete]({{ site.url }}/images/posts/2016.04.18/ospf-network-diagram.png){:.center-image}

**<center>Addressing Table</center>**
![Addressing Table Complete]({{ site.url }}/images/posts/2016.04.18/ospf-address-table-complete.png){:.center-image}

**Enable OSPF with Process ID 1**

{% highlight bash %}
Bldg-1(config)#router ospf 1
{% endhighlight bash %}

**OSPF Router ID**

{% highlight bash %}
Bldg-1(config)#router ospf 1
Bldg-1(config-router)#router-id 1.1.1.1
{% endhighlight bash %}

**OSPF Link Cost**

{% highlight bash %}
Bldg-1(config)#int s0/0/0
Bldg-1(config-if)#ip ospf cost 7500
{% endhighlight bash %}

**OSPF Network assignment**

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

**Prevent Routing updates being sent to LAN**

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

**OSPF MD5 Authentication**

{% highlight bash %}
- Use a key value of 1
- Use xyz_OSPF as the password
- Apply MD5 authentication to the required interfaces

Bldg-1(config)#int s0/0/0
Bldg-1(config-if)#ip ospf message-digest-key 1 md5 xyz_OSPF
Bldg-1(config-if)#ip ospf authentication message-digest

Main(config)#int s0/1/0
Main(config-if)#ip ospf message-digest-key 1 md5 xyz_OSPF
Main(config-if)#ip ospf authentication message-digest
{% endhighlight bash %}

***

#### <center>6. EIGRP Configuration</center>

***

**<center>EIGRP Sample Topology</center>**
![EIGRP Routing Table]({{ site.url }}/images/posts/2016.04.18/eigrp-sample-topology.png){:.center-image}

**<center>EIGRP Address Table</center>**
![EIGRP Routing Table]({{ site.url }}/images/posts/2016.04.18/eigrp-address-table.png){:.center-image}

**Display the directly connected networks**

{% highlight bash %}
R1(config-router)#do show ip route
{% endhighlight bash %}

![EIGRP Routing Table]({{ site.url }}/images/posts/2016.04.18/eigrp-routing-table.png){:.center-image}

**Configure EIGRP to advertise to directly connected networks**

{% highlight bash %}
R1(config-router)#network 172.16.1.0 0.0.0.255
R1(config-router)#network 172.16.3.0 0.0.0.3
R1(config-router)#network 192.168.10.4 0.0.0.3

R2(config-router)#network 172.16.2.0 0.0.0.255
R2(config-router)#network 172.16.3.0 0.0.0.3
R2(config-router)#network 192.168.10.8 0.0.0.3

R3(config-router)#network 192.168.1.0 0.0.0.255
R3(config-router)#network 192.168.10.4 0.0.0.3
R3(config-router)#network 192.168.10.8 0.0.0.3
{% endhighlight bash %}

**Configure Passive Interface (Prevent Updates sent to LAN)**

{% highlight bash %}
R1(config-router)#passive-interface g0/0
R2(config-router)#passive-interface g0/0
R3(config-router)#passive-interface g0/0
{% endhighlight bash %}

**Disable Auto Summary**

{% highlight bash %}
R1(config)#router eigrp 1
R1(config-router)#no auto-summary
{% endhighlight bash %}

**Verify EIGRP Routing**

{% highlight bash %}
R1#show ip eigrp neighbors
{% endhighlight bash %}

![EIGRP Routing neighbors]({{ site.url }}/images/posts/2016.04.18/eigrp-route-neighbours.png){:.center-image}

**Manual Summary Calculations**

![EIGRP Topology]({{ site.url }}/images/posts/2016.04.18/eigrp-topology.png){:.center-image}

![EIGRP Address]({{ site.url }}/images/posts/2016.04.18/eigrp-addresses.png){:.center-image}

**Manual Summary Address - Example 1**

1. Find the Last place a common bit pattern occurs in the 4 octets. This will be our summary

![EIGRP Summary 1]({{ site.url }}/images/posts/2016.04.18/eigrp-summary-1.png){:.center-image}

{% highlight bash %}
Branch-1(config)#int s0/0/0
Branch-1(config-if)#ip summary-address eigrp 1 172.31.8.0 255.255.252.0
{% endhighlight bash %}

**Manual Summary Address - Example 2**

![EIGRP Summary 2]({{ site.url }}/images/posts/2016.04.18/eigrp-summary-2.png){:.center-image}

{% highlight bash %}
Branch-2(config-if)#int s0/0/1
Branch-2(config-if)#ip summary-address eigrp 1 172.31.12.0 255.255.252.0
{% endhighlight bash %}

**Manual Summary of the Previous Examples**

![EIGRP Summary 3]({{ site.url }}/images/posts/2016.04.18/eigrp-summary-3.png){:.center-image}

{% highlight bash %}
IPv4-Edge(config)#int s0/1/0
IPv4-Edge(config-if)#ip summary-address eigrp 1 172.31.8.0 255.255.248.0
{% endhighlight bash %}

**IPv6 Manual Summary - Example 1**

![EIGRP Summary 4]({{ site.url }}/images/posts/2016.04.18/eigrp-summary-4.png){:.center-image}

{% highlight bash %}
Branch-3(config)#ipv6 router eigrp 1
Branch-3(config-rtr)#eigrp router-id 3.3.3.3
Branch-3(config-rtr)#int s0/0/0
Branch-3(config-if)#ipv6 summary-address eigrp 1 2001:DB8:1:8::/62
{% endhighlight bash %}

**IPv6 Manual Summary - Example 2**

![EIGRP Summary 5]({{ site.url }}/images/posts/2016.04.18/eigrp-summary-5.png){:.center-image}

{% highlight bash %}
Branch-4(config)#ipv6 router eigrp 1
Branch-4(config-rtr)#eigrp router-id 4.4.4.4
Branch-4(config-rtr)#int s0/0/1
Branch-4(config-if)#ipv6 summary-address eigrp 1 2001:DB8:1:C::/62
{% endhighlight bash %}

**IPv6 Manual Summary of the Previous Examples**

![EIGRP Summary 6]({{ site.url }}/images/posts/2016.04.18/eigrp-summary-6.png){:.center-image}

{% highlight bash %}
HQ-IPv6(config)#ipv6 router eigrp 1
HQ-IPv6(config-rtr)#eigrp router-id 1.1.1.1
HQ-IPv6(config)#int s0/0/1
HQ-IPv6(config-if)#ipv6 summary-address eigrp 1 2001:DB8:1:8::/61
{% endhighlight bash %}

***

#### <center>7. VLANs and Trunking</center>

***

**Creating VLANs**

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
{% endhighlight bash %}

**Assign Switch Ports to VLANs**

{% highlight bash %}
FL-A(config)#interface fa0/5
FL-A(config-if)#switchport access vlan 2
FL-A(config-if)#no shutdown

FL-A(config)#interface fa0/10
FL-A(config-if)#switchport access vlan 4
FL-A(config-if)#no shutdown

FL-A(config-if)#switchport access vlan 8
FL-A(config-if)#no shutdown

FL-A(config)#interface fa0/24
FL-A(config-if)#switchport access vlan 15
FL-A(config-if)#no shutdown
{% endhighlight bash %}

**Switchport Static Access**

{% highlight bash %}
FL-A(config)#interface fa0/5
FL-A(config-if)#switchport mode access

FL-A(config)#interface fa0/10
FL-A(config-if)#switchport mode access

FL-A(config)#interface fa0/15
FL-A(config-if)#switchport mode access

FL-A(config)#interface fa0/24
FL-A(config-if)#switchport mode access
{% endhighlight bash %}

**Sub-interface configuration**
**Defines the encapsulation format as IEEE 802.1Q (dot1q), and specifies the VLAN identifier**
**Specifies the VLAN identifier**

{% highlight bash %}
PoliceDept(config)#int g0/0
PoliceDept(config-if)#no shutdown
PoliceDept(config-if)#int g0/0.45
PoliceDept(config-subif)#encapsulation dot1Q 45
PoliceDept(config-subif)#ip address 192.168.45.1 255.255.255.0
PoliceDept(config-subif)#int g0/0.47
PoliceDept(config-subif)#encapsulation dot1Q 47
PoliceDept(config-subif)#ip address 192.168.47.1 255.255.255.0
PoliceDept(config-subif)#int g0/0.101
PoliceDept(config-subif)#encapsulation dot1Q 101
PoliceDept(config-subif)#ip address 192.168.101.1 255.255.255.0
{% endhighlight bash %}

***

#### <center>8. EtherChannel and Trunking</center>

***

**Channel 1 and 2 initiate negotiations**

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

**Channel 3 side B should negotiiate with side C**

{% highlight bash %}
FL-B(config)#interface range fa0/5-6
FL-B(config-if-range)#channel-group 3 mode active
FL-B(config-if-range)#no shutdown

FL-B(config)#interface port-channel 3
FL-B(config-if)#switchport mode trunk
{% endhighlight bash %}

**Channel 3 side C should not initiate negotiations with B**

{% highlight bash %}
FL-C(config)#interface range fa0/5-6
FL-C(config-if-range)#channel-group 3 mode passive
FL-B(config-if-range)#no shutdown

FL-C(config)#interface port-channel 3
FL-C(config-if)#switchport mode trunk
{% endhighlight bash %}

**Configure Static Trunking on switchport**

{% highlight bash %}
FL-B(config)#interface g0/1
FL-B(config-if)#switchport mode trunk
{% endhighlight bash %}

***

#### <center>9. Switch Security</center>

***

**Configure port security on all active access ports**

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

**Accept only two MAC addresses**

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

**MAC addresses should be recorded**

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

**Switchports should provide notification, but not place interface in disabled state**

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

***

#### <center>10. Configure DHCP</center>

***

**DHCP Pool Creation**

{% highlight bash %}
Bldg-2(config)#ip dhcp pool vlan2pool
Bldg-2(dhcp-config)#network 10.10.2.0 255.255.255.0
Bldg-2(dhcp-config)#default-router 10.10.2.1
Bldg-2(dhcp-config)#dns-server 192.168.200.225
{% endhighlight bash %}

**Exclude the first five addresses from pool**

{% highlight bash %}
Bldg-2(config)#ip dhcp excluded-address 10.10.2.1 10.10.2.5
Bldg-2(config)#ip dhcp excluded-address 10.10.4.1 10.10.4.5
Bldg-2(config)#ip dhcp excluded-address 10.10.8.1 10.10.8.5
{% endhighlight bash %}

***

#### <center>11. Configure Access Control Lists</center>

***

**Create a named standard ACL using the name MANAGE**

{% highlight bash %}
Central(config)#ip access-list standard MANAGE
{% endhighlight bash %}

**Allow only the host on 203.0.113.18 access**

{% highlight bash %}
Central(config-std-nacl)#permit host 203.0.113.18
{% endhighlight bash %}

**Apply this policy to the VTY lines**

{% highlight bash %}
Central(config)#line vty 0 4
Central(config-line)#password class
Central(config-line)#login
Central(config-line)#access-class MANAGE in
{% endhighlight bash %}

**Create an Access list with number 101**
**Allow external host 203.0.113.18 full access to inside network**

{% highlight bash %}
Central(config)#access-list 101 permit ip host 203.0.113.18 any
{% endhighlight bash %}

**Allow outside access to 198.51.100.14 over HTTP only**

{% highlight bash %}
Central(config)#access-list 101 permit tcp any host 198.51.100.14 eq www
{% endhighlight bash %}

**Allow responses to data requests to enter the Network**

{% highlight bash %}
Central(config)#access-list 101 permit tcp any any established
Central(config)#access-list 101 deny ip any any
{% endhighlight bash %}

**Activate access list on interface**

{% highlight bash %}
Central(config)#int s0/1/0
Central(config-if)#ip access-group 101 in
{% endhighlight bash %}

***

#### <center>12. Spanning Tree Protocol</center>

***

**Activate Rapid PVST+ and set root priorities**

{% highlight bash %}
FL-A(config)#spanning-tree mode rapid-pvst
FL-B(config)#spanning-tree mode rapid-pvst
FL-C(config)#spanning-tree mode rapid-pvst
{% endhighlight bash %}

**FL-A should be configured as root primary for VLAN 2 and VLAN 4 using the default primary priority values**

{% highlight bash %}
FL-A(config)#spanning-tree vlan 2 root primary
FL-A(config)#spanning-tree vlan 4 root primary
{% endhighlight bash %}

**FL-A should be configured as root secondary for VLAN 8 and VLAN 15 using the default secondary priority values**

{% highlight bash %}
FL-A(config)#spanning-tree vlan 8 root secondary
FL-A(config)#spanning-tree vlan 15 root secondary
{% endhighlight bash %}

**FL-C should be configured as root primary for VLAN 8 and VLAN 15 using the default primary priority values**

{% highlight bash %}
FL-C(config)#spanning-tree vlan 8 root primary
FL-C(config)#spanning-tree vlan 15 root primary
{% endhighlight bash %}

**FL-C should be configured as root secondary for VLAN 2 and VLAN 4 using the default secondary priority values**

{% highlight bash %}
FL-C(config)#spanning-tree vlan 2 root secondary
FL-C(config)#spanning-tree vlan 4 root secondary
{% endhighlight bash %}

**Activate PortFast and BPDU Guard on the active FL-C switch access ports**
**On FL-C, configure PortFast on the access ports that are connected to hosts**

{% highlight bash %}
FL-C(config)#interface range fa0/7, fa0/10, fa0/15, fa0/24
FL-C(config-if-range)#spanning-tree portfast
{% endhighlight bash %}

**On FL-C, activate BPDU Guard on the access ports that are connected to hosts**

{% highlight bash %}
FL-C(config)#interface range fa0/7, fa0/10, fa0/15, fa0/24
FL-C(config-if-range)#spanning-tree bpduguard enable
FL-C(config-if-range)#no shutdown
{% endhighlight bash %}

***

#### <center>13. NAT</center>

***

**Translate the internal address of the server to the address 198.51.100.14**

{% highlight bash %}
Central(config)#ip nat inside source static 192.168.18.46 198.51.100.14
{% endhighlight bash %}

**Configure the correct interfaces to perform this NAT translation**

{% highlight bash %}
Central(config)#int g0/0
Central(config-if)#ip nat inside
Central(config-if)#int s0/1/0
Central(config-if)#ip nat outside
{% endhighlight bash %}

**Configure Dynamic NAT, Use a pool name of INTERNET**

{% highlight bash %}
Central(config)#ip nat pool INTERNET 198.51.100.3 198.51.100.13 netmask 255.255.255.240
{% endhighlight bash %}

**Hosts on LAN can use Internet, source list number 1**

{% highlight bash %}
Central(config)#access-list 1 permit 192.168.45.0 0.0.0.255
Central(config)#access-list 1 permit 192.168.47.0 0.0.0.255
Central(config)#access-list 1 permit 192.168.200.0 0.0.3.255
Central(config)#ip nat inside source list 1 pool INTERNET
{% endhighlight bash %}
