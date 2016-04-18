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
  * Static Route to neighbour network

5. **OSPF Configuration**
  * Enable OSPF with Process ID 1
  * OSPF Router ID
  * OSPF Link Cost
  * OSPF Network assignment
  * Prevent Routing updates being sent to LAN
  * OSPF MD5 Authentication

6. **VLANs and Trunking**
  * Creating VLANs
  * Assign Switch Ports to VLANs
  * Switchport Static Access

7. **EtherChannel and Trunking**
  * Channel 1 and 2 initiate negotiations
  * Channel 3 side B should negotiiate with side C
  * Channel 3 side C should not initiate negotiations with B
  * Configure Static Trunking on switchport

8. **Switch Security**
  * Configure port security on all active access ports
  * Accept only two MAC addresses
  * MAC addresses should be recorded
  * Switchports should provide notification, but not place interface in disabled state.

9. **Configure DHCP**
  * DHCP Pool Creation
  * Exclude the first five addresses from pool.

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

#### <center>6. VLANs and Trunking</center>

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

***

#### <center>7. EtherChannel and Trunking</center>

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

#### <center>8. Switch Security</center>

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

#### <center>9. Configure DHCP</center>

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
