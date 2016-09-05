---
layout: post
title: CCNAS - Practice Skills Assessment
date: "2016-09-05"
comments: true
description: A general overview of skills required for the CCNAS Practical Assessment
share: true
author: nathan
tags:
 - ccnas
---

## <center>Contents</center>

---

1. **Verify Basic Device Settings**
  * 1.1. Initialize and reload routers
2. **Configure Secure Router Administrative Access**
  * 2.1. Configure encrypted passwords and a login banner
  * 2.2. Configure EXEC timeout on console and VTY lines
  * 2.3. Configure login failure rates and VTY login enhancements
  * 2.4. Configure SSH access and disable Telnet
  * 2.5. Configure RADIUS/TACACS+/Local AAA authentication
3. **Configure a Site-to-Site VPN between ISRs**
  * 3.1. Configure an IPsec site-to-site VPN between R1 and R3 using CCP
4. **Configure an ISR firewall and Intrusion Prevention System**
  * 4.1. Configure a zone-based policy (ZPF) firewall on an ISR using CCP
  * 4.2. Configure an Intrusion Prevention System (IPS) on an ISR using CCP
5. **Secure Network Switches**
  * 5.1. Configure passwords and a login banner
  * 5.2. Configure management VLAN access
  * 5.3. Secure trunk ports
  * 5.4. Secure access ports
  * 5.5. Protect against STP attacks
6. **Configure ASA Basic Settings and Firewall**
  * 6.1. Configure basic settings, passwords, date and time
  * 6.2. Configure the inside and outside VLAN interfaces
  * 6.3. Configure port address translation (PAT) for the inside network
  * 6.4. Configure a DHCP server for the inside network
  * 6.5. Configure administrative access via Telnet and SSH
  * 6.6. Configure a static default route for the ASA
  * 6.7. Configure RADIUS/TACACS+/Local AAA user authentication
  * 6.8. Verify address translation and firewall functionality
7. **Configure ASA AnyConnect SSL VPN Remote Access**
  * 7.1. Configure a remote access AnyConnect SSL VPN using ASDM
  * 7.2. Verify AnyConnect SSL VPN access to the portal

---

## <center>Topology</center>

---

![Topology]({{ site.url }}/images/posts/2016.09.05/ccnas-practice-skills-topology.png){:.center-image}

---

## <center>Addressing Table</center>

---

![Topology]({{ site.url }}/images/posts/2016.09.05/ccnas-practice-skills-addressing-table.png){:.center-image}

---

## <center>1. Verify Basic Device Settings</center>

---

### **Initialize and reload routers**

#### Basic settings for all routers configured.

> Host names, interface IP addresses, serial interface DCE clock rate

> Host names and interface IP addresses >> Refer to table above!

{% highlight bash %}
R1(config)#interface S0/0/0
R1(config-if)#clock rate 64000

R2(config)#interface S0/0/1
R2(config-if)#clock rate 64000
{% endhighlight bash %}

> DNS lookup disabled on each router.

{% highlight bash %}
R1(config)#no ip domain-lookup
R2(config)#no ip domain-lookup
R3(config)#no ip domain-lookup
{% endhighlight bash %}

#### Static default routes on routers R1, R2 and R3 configured.

> Static default route from R1 to R2 and from R3 to R2.

{% highlight bash %}
R1(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.2
R3(config)#ip route 0.0.0.0 0.0.0.0 10.20.20.2
{% endhighlight bash %}

> Static routes from R2 to the R1 simulated LAN (Loopback 1), the R1 Fa0/0-to-ASA subnet and the R3 LAN.

{% highlight bash %}
R2(config)#ip route 10.10.10.2 255.255.255.0 172.20.1.1
R1(config)#ip route 209.165.200.233 255.255.255.248 172.30.3.1
{% endhighlight bash %}

#### Basic settings for each switch configured.

> Host names, VLAN 1 management address, IP default gateway.

{% highlight bash %}
S1(config)#interface vlan 1
S1(config)#ip address 192.168.10.11 255.255.255.0
S1(config)#no shutdown
S1(config)#ip default-gateway 192.168.10.1

S2(config)#interface vlan 1
S2(config)#ip address 192.168.10.12 255.255.255.0
S2(config)#no shutdown
S2(config)#ip default-gateway 192.168.10.1

S3(config)#interface vlan 1
S3(config)#ip address 172.30.3.11 255.255.255.0
S3(config)#no shutdown
S3(config)#ip default-gateway 172.30.3.1
{% endhighlight bash %}

> DNS lookup disabled on each switch.
{% highlight bash %}
S1(config)#no ip domain-lookup
S2(config)#no ip domain-lookup
S3(config)#no ip domain-lookup
{% endhighlight bash %}

#### PC host IP settings configured.

> Static IP address, subnet mask, and default gateway for each PC.

{% highlight bash %}
>>> refer to the table above!
{% endhighlight bash %}

#### Verify connectivity between PC-C and R1 Lo1 and Fa0/0.

> On PC-C
{% highlight bash %}
>> ping 172.20.1.1 (loopback 1)
>> ping 209.165.200.233 (R1 Fa0/0)
{% endhighlight bash %}

---

## <center>2. Configure Secure Router Administrative Access</center>

---

### **Task 1: Configure Settings for R1 and R2.**

#### Step 1: Configure a minimum password length of 10 characters on R1.

{% highlight bash %}
R1(config)#security passwords min-length 10
{% endhighlight bash %}

#### Step 2: Configure the enable secret password on R1.

> Use an enable secret password of ciscoenapa55.

{% highlight bash %}
R1(config)#enable secret ciscoenapa55
{% endhighlight bash %}

#### Step 3: Encrypt plaintext passwords on R1.

{% highlight bash %}
R1(config)#service password-encryption
{% endhighlight bash %}

#### Step 4: Configure the console lines on R1.

> Configure a console password of ciscoconpa55 and enable login. Set the exec-timeout to log out after
  15 minutes of inactivity. Prevent console messages from interrupting command entry. Note: The vty lines for R1 are configured for SSH in Task 3.

{% highlight bash %}
R1(config)#line console 0
R1(config-line)#password ciscoconpa55
R1(config-line)#exec-timeout 15 0
R1(config-line)#login
R1(config-line)#logging synchronous
{% endhighlight bash %}

#### Step 5: Configure a login warning banner on R1.

> Configure a warning to unauthorized users with a message-of-the-day (MOTD) banner that says: “Unauthorized access strictly prohibited and prosecuted to the full extent of the law!”.

{% highlight bash %}
R1(config)#banner motd $Unauthorized access strictly prohibited and prosecuted to the full extent of the law!$
{% endhighlight bash %}

#### Step 6: Configure the vty lines and enable password on R2.

> Configure the password for vty lines to be ciscovtypa55 and enable login. Set the exec-timeout so a
  session is logged out after 15 minutes of inactivity.

{% highlight bash %}
R2(config)#line vty 0 4
R2(config-line)#password ciscovtypa55
R2(config-line)#exec-timeout 15 0
R2(config-line)#login
{% endhighlight bash %}

> Use an enable secret password of ciscoenapa55.

{% highlight bash %}
R2(config)#enable secret ciscoenapa55
{% endhighlight bash %}

#### Step 7: Enable HTTP access on router R2.

> Enable the HTTP server on R2 to simulate an Internet target for later testing.

{% highlight bash %}
R2(config)#ip http server
{% endhighlight bash %}

### **Task 2: Configure Local Authentication with AAA on R1.**

#### Step 1: Configure the local user database.

> Create a local user account of Admin01 with a secret password of Admin01pa55 and a privilege level of 15.

{% highlight bash %}
R1(config)#username Admin01 privilege 15 secret Admin01pa55
{% endhighlight bash %}

#### Step 2: Enable AAA services.

{% highlight bash %}
R1(config)#aaa new-model
{% endhighlight bash %}

#### Step 3: Implement AAA services using RADIUS/TACACS+/local database.

> Create the default login authentication method list using RADIUS as the first option, TACACS+ as the second option, and case-sensitive local authentication as the third option and the enable password as the backup option to use if an error occurs in relation to local authentication.

{% highlight bash %}
R1(config)#aaa authentication login default group radius enable
R1(config)#aaa authentication login default group tacacs+ enable
R1(config)#aaa authentication login default local enable
{% endhighlight bash %}

### **Task 3: Configure the SSH Server on Router R1.**

#### Step 1: Configure the domain name ccnasecurity.com.

{% highlight bash %}
R1(config)#ip domain-name ccnasecurity.com
{% endhighlight bash %}

#### Step 2: Configure the incoming vty lines.

> Specify that the router vty lines will accept only SSH connections.

{% highlight bash %}
R1(config)#line vty 0 4
R1(config-line)#transport input ssh
R1(config-line)#exit
{% endhighlight bash %}

#### Step 3: Generate the RSA encryption key pair.

> Configure the RSA keys with 1024 as the number of modulus bits.

{% highlight bash %}
R1(config)#crypto key generate rsa general-keys modulus 1024
{% endhighlight bash %}

#### Step 4: Configure the SSH version.

> Specify that the router accept only SSH version 2 connections.

{% highlight bash %}
R1#show ip ssh
{% endhighlight bash %}

#### Step 5: Verify SSH connectivity to R1 from PC-C.

> Launch the SSH client on PC-C and test SSH connectivity to R1 and login in as Admin01 with the
  password Admin01pa55.

{% highlight bash %}
• Server: ccnasecurity.com
• Port: 22
• Username: Admin01
• Password: Admin01pa55
{% endhighlight bash %}

### **Task 4: Secure against login attacks on R1.**

#### Step 1: Configure enhanced login security on R1.

> If a user fails to login twice within a 30 second time span, then disable logins for 1 minute. Log all failed login attempts.

{% highlight bash %}
R1(config)#login on-failure log
{% endhighlight bash %}

---

## <center>3. Configure a Site-to-Site IPsec VPN between ISRs</center>

---

> In Part 3 of this SBA, you use CCP to configure an IPsec VPN tunnel between R1 and R3 that passes through R2.

### **Task 1: Configure the site-to-site VPN between R1 and R3.**

#### Step 1: Configure the enable secret password and HTTP access on R3 prior to starting CCP.

> From the CLI, configure an enable secret password of ciscoenapa55 for use with CCP on R3.

{% highlight bash %}
R3(config)#enable secret ciscoenapa55
{% endhighlight bash %}

> Enable the HTTP server on R3.

{% highlight bash %}
R3(config)# ip http server
{% endhighlight bash %}

> Add user Admin01 to the local database with a privileged level of 15, and a password of Admin01pa55.

{% highlight bash %}
R3(config)# username Admin01 privilege 15 secret Admin01pa55
{% endhighlight bash %}

> Configure local database authentication of HTTP sessions.

{% highlight bash %}
R3(config)# ip http authentication local
{% endhighlight bash %}

#### Step 2: Access CCP and discover R3.

> From PC-C run CCP and access R3.

{% highlight bash %}
• Manage Devices window > R3 IP address 172.30.3.1 in the first IP address field.
• Enter Admin01 in the Username field, and Admin01pa55 in the Password field.
• At the CCP Dashboard, click the Discover button to discover and connect to R3.
{% endhighlight bash %}

#### Step 3: Use the CCP VPN wizard to configure R3.

> Use the Quick Setup option to configure the R3 side of the site-to-site VPN.

{% highlight bash %}
• Click the Configure button at the top of the CCP screen.
• Choose Security > VPN > Site-to-Site VPN.
• Click the Launch the selected task button to begin the CCP Site-to-Site VPN wizard.
• From the initial Site-to-Site VPN wizard screen, choose the Step by step wizard, and then click Next.
{% endhighlight bash %}

#### Step 4: Configure basic VPN connection information settings.

> Specify R3 S0/0/1 as the interface for the connection and R1 interface S0/0/0 as the remote peer static IP address.

{% highlight bash %}
• On the VPN Connection Information screen, select the interface for the connection, which should be R3 Serial0/0/1.
• In the Peer Identity section, select Peer with static IP address and enter the IP address of the remote peer, R1 interface S0/0/0, which is 10.10.10.1.
{% endhighlight bash %}

> Specify the pre-shared VPN key cisco12345.

{% highlight bash %}
• In the Authentication section, click Pre-shared Keys, and enter the pre-shared VPN key cisco12345. Re-enter the key for confirmation. Click Next to continue.
{% endhighlight bash %}

> Encrypt traffic between the R3 LAN and the R1 Loopback 1 simulated LAN.

{% highlight bash %}
• On the IKE Proposals screen, click Next to continue.
• On the Transform Set screen, click Next to continue.
• On the Traffic to protect screen, enter the following information;
{% endhighlight bash %}

{% highlight bash %}
Local Network (R3 LAN) : IP address: 172.30.3.1 Subnet Mask: 255.255.255.0

Remote Network (R1 Loopback) : IP address: 172.20.1.1 Subnet Mask: 255.255.255.0
{% endhighlight bash %}

{% highlight bash %}
• Click Next to continue.
• Review the Summary of the Configuration screen. You can scroll down to see the IPsec rule (ACL) that CCP creates for R3, which permits all traffic from network 172.30.3.1/24 to network 172.20.1.1/24.
• Click Finish to go to the Deliver Configuration to Device screen.
• On the Deliver Configuration to Device screen, select Save running config. to device’s startup config, and click the Deliver button.
• After the commands have been delivered, click OK.
{% endhighlight bash %}

{% highlight bash %}
To save these configuration commands for later editing or documentation purposes;
• Click Save to file button.
{% endhighlight bash %}

#### Step 5: Generate a mirror configuration from R3 and apply it to R1.

> On R3, generate a mirror configuration for application to router R1 and save it to the desktop or flash drive. Edit the file as necessary and apply the mirrored commands to R1.

{% highlight bash %}
On R3

• Click the Configure button at the top of the CCP screen.
• Choose Security > VPN > Site-to-Site VPN. Click the Edit Site to Site VPN tab.
• Select the VPN policy you just configured on R1 and click the Generate Mirror button in the lower right of the window.

The Generate Mirror window displays the commands necessary to configure R3 as a VPN peer. Scroll through the window to see all the commands generated.

• Click the Save button to create a text file. Name it VPN-Mirror-Cfg-for-R3.txt.
{% endhighlight bash %}

> Apply the crypto map to the R1 VPN interface.

{% highlight bash %}
• On R1, enter privileged EXEC mode and then global config mode.
• Copy the commands from the text file into the R1 CLI.

To apply the crypto map to R1 VPN interface, enter the following;

R1(config)#interface S0/0/0
R1(config-if)#crypto map SDM_CMAP_1
{% endhighlight bash %}

### **Task 2: Test the Site-to-Site IPsec VPN using CCP**

> On R3 (PC-C), use CCP to test the IPsec VPN tunnel between the two routers.

{% highlight bash %}
• Security > VPN > Site-to-Site VPN and click the Edit Site-to-Site VPN tab.
• From the Edit Site to Site VPN tab, choose the VPN and click Test Tunnel.
• VPN Troubleshooting window displays, click the Start button to start troubleshooting the tunnel.
• Click Yes to continue when the CCP Warning window is displayed.
• In the next VPN Troubleshooting window, enter the IP address of the R3 Fa0/1 interface in the ∂destination network field (172.30.3.1) and click Continue to begin the debugging process.
• If successful, click Save Report. Click Close when you’re finished.
{% endhighlight bash %}

> Ping from PC-C to the R1 Lo1 interface at 172.16.1.1 to generate some interesting traffic.

{% highlight bash %}
On PC-C >> ping 172.16.1.1
{% endhighlight bash %}

> Issue the show crypto isakmp sa command on R3 to view the security association created.

{% highlight bash %}
R3#show crypto isakmp sa
{% endhighlight bash %}

> Issue the show crypto ipsec sa command on R1 to verify packets are being received from R3 and decrypted by R1.

{% highlight bash %}
R1#show crypto isakmp sa
{% endhighlight bash %}

---

## <center>4. Configure an ISR Firewall and Intrusion Prevention System</center>

---

> In Part 4, you configure a zone-based policy firewall and IPS on R3 using CCP.

### **Task 1: Configure a ZPF Firewall on R3 using CCP.**

#### Step 1: Use the CCP Firewall wizard to configure a zone-based firewall.

> Configure a Basic firewall with Fa0/1 interface as the Inside interface and S0/0/1 as the Outside interface.

{% highlight bash %}
• Configure > Security > Firewall > Firewall. Select Basic Firewall.
• Click the Launch the selected task button. Click Next to continue.
• Check the Inside (trusted) check box for Fast Ethernet0/1 and the Outside (untrusted) check box for Serial0/0/1. Click Next.
• Click OK when the warning is displayed informing you that you cannot launch CCP from the S0/0/1 interface after the Firewall wizard completes.
{% endhighlight bash %}

> Use the Low Security setting, and complete the Firewall wizard.

{% highlight bash %}
• Move the slider to Low Security and click the Preview Commands button to preview the commands that are delivered to the router. Click Next to continue.
• On the Review the Firewall Configuration Summary screen, click Finish to complete the Firewall wizard.
{% endhighlight bash %}

#### Step 2: Verify Firewall functionality.

> From PC-C, ping external router R2. The pings should be successful.

{% highlight bash %}
>> ping 10.20.20.2
{% endhighlight bash %}

> From external router R2, ping PC-C. The pings should NOT be successful.

{% highlight bash %}
>> ping 172.30.3.3
{% endhighlight bash %}

### **Task 2: Configure IPS on R3 Using CCP.**

#### Step 1: Prepare router R3 and the TFTP server.

> To configure Cisco IOS IPS 5.x, the IOS IPS signature package file and public crypto key files must be available on the PC with the TFTP server installed. R3 uses PC-C as the TFTP server. Check with your instructor if these files are not on the PC.

{% highlight bash %}
• Install TFTP Server application on PC-C
{% endhighlight bash %}

> Verify that the IOS-Sxxx-CLI.pkg signature package file is in the default TFTP folder. The xxx is the version number and varies depending on which file was downloaded from Cisco.com.

> Verify that the realm-cisco.pub.key.txt file is available and note its location on PC-C.

> Verify or create the IPS directory, ipsdir, in router flash on R3.

{% highlight bash %}
R3#mkdir ipsdir
R3#dir flash:
{% endhighlight bash %}

> Note: For router R3, the IPS signature (.xml) files in the flash:/ipsdir/ directory should have been deleted and the directory removed prior to starting the SBA. The files must be deleted from the directory in order to remove it.

> Note: If the ipsdir directory is listed and there are files in it, contact your instructor. This directory must be empty before configuring IPS. If there are no files in it you may proceed to configure IPS.

#### Step 2: Access CCP and discover R3 (if required).

> Specify Admin01 as the username and Admin01pa55 as the password.

{% highlight bash %}
• In the Manage Devices window add R3 IP address 172.30.3.1 in the first IP address field.
• Enter Admin01 in the Username field, and Admin01pa55 in the Password field.
• At the CCP Dashboard, click the Discover button to discover and connect to R3. If discovery fails, click the Discovery Details button to determine the problem.
{% endhighlight bash %}

#### Step 3: Use the CCP IPS wizard to configure IPS.

> Launch the IPS wizard and apply the IPS rule in the inbound direction for Serial0/0/1.

{% highlight bash %}
• Click the Configure button at the top of the CCP screen.
• Choose Security > Intrusion Prevention > Create IPS.
• Click Launch IPS Rule Wizard. Click Next to continue.
• In the Select Interfaces window, check the Inbound check box for Fast Ethernet0/1 and Serial0/0/1. Click Next.
{% endhighlight bash %}

> Specify the signature file with a URL and use TFTP to retrieve the file from PC-C.

{% highlight bash %}
• Signature File and Public Key window, click the ellipsis (...) button next to Specify the Signature File You Want to Use with IOS IPS to open the Specify Signature File window. Confirm that the Specify signature file using URL option is chosen.
• For Protocol, select tftp from the drop-down menu.
• Enter the IP address of the PC-C TFTP server and the filename. The address is 172.30.3.3/IOS-Sxxx-CLI.pkg (where xxx is the number of the package)
• Click OK to return to the Signature File and Public Key window.
{% endhighlight bash %}

> Name the public key file realm-cisco.pub.

{% highlight bash %}
• In the Configure Public Key section of the Signature File and Public Key window, enter realm-cisco.pub in the Name field.
{% endhighlight bash %}

> Copy the text from the public key file to the CCP IPS wizard.

{% highlight bash %}
• Open the realm-cisco-pub-key.txt file located on PC-C.
• Copy the text between the phrase key-string and the word quit into the Key field in the Configure Public Key section.
• Click Next to display the Config Location and Category window.
{% endhighlight bash %}

> Specify the flash:/ipsdir/ directory name as the location to store the signature information.

{% highlight bash %}
• In the Config Location and Category window in the Config Location section, click the ellipsis (...) button next to Config Location to add the location.
• Verify that Specify the config location on this router is selected. Click the ellipsis (...) button.

Click the plus sign (+) next to flash. Choose ipsdir and then click OK.
{% endhighlight bash %}

> Choose the basic category.

{% highlight bash %}
• In the Choose Category field of the Config Location and Category window, choose basic.
{% endhighlight bash %}

> Complete the wizard.

{% highlight bash %}
• Click Next in the Cisco CCP IPS Policies Wizard window.
• Click Finish in the IPS Policies Wizard window and review the commands that will be delivered to the router. • Click Deliver.
• Click OK when the Commands Deliver Status window is ready.
• When the signature configuration process has completed, you return to the IPS window with the Edit IPS tab selected.
{% endhighlight bash %}

---

## <center>5. Secure Network Switches</center>

---

### **Task 1: Configure Passwords and a Login Banner on S1.**

#### Step 1: Configure the enable secret password of ciscoenapa55.

{% highlight bash %}
S1(config)#enable secret ciscoenapa55
{% endhighlight bash %}

#### Step 2: Encrypt plaintext passwords.

{% highlight bash %}
S1(config)#service password-encryption
{% endhighlight bash %}

#### Step 3: Configure the console and VTY lines.

> Configure a console password of ciscoconpa55 and set the exec-timeout to log out after 5 minutes of inactivity. Prevent console messages from interrupting command entry.

{% highlight bash %}
S1(config)#line console 0
S1(config-line)#password ciscoconpa55
S1(config-line)#exec-timeout 5 0
S1(config-line)#login
S1(config-line)#logging synchronous
{% endhighlight bash %}

> Configure a vty lines password of ciscovtypa55 and set the exec-timeout to log out after 5 minutes of inactivity.

{% highlight bash %}
S1(config)#line vty 0 15
S2(config-line)#password ciscovtypa55
S2(config-line)#exec-timeout 5 0
S2(config-line)#login
{% endhighlight bash %}

#### Step 4: Configure a login warning banner.

> Configure a warning to unauthorized users with a message-of-the-day (MOTD) banner that says “Unauthorized access strictly prohibited and prosecuted to the full extent of the law!”.

{% highlight bash %}
S1(config)#banner motd $Unauthorized access strictly prohibited and prosecuted to the full extent of the law!$
{% endhighlight bash %}

#### Step 5: Disable HTTP access.

{% highlight bash %}
S1(config)#no http server
{% endhighlight bash %}

### **Task 2: Secure Trunk and Access Ports on S1 and S2.**

#### Step 1: Configure trunk ports on S1 and S2.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#switchport mode trunk
S2(config)#interface FastEthernet 0/1
S2(config-if)#switchport mode trunk
{% endhighlight bash %}

#### Step 2: Change the native VLAN to 99 for the trunk ports on S1 and S2.

{% highlight bash %}
S1(config)#interface Fa0/1
S1(config-if)#switchport trunk native vlan 99
S1(config-if)#end
S2(config)#interface Fa0/1
S2(config-if)#switchport trunk native vlan 99
S2(config-if)#end
{% endhighlight bash %}

#### Step 3: Prevent the use of DTP on S1 and S2 trunk ports.

{% highlight bash %}
S1(config)#interface Fa0/1
S1(config-if)#switchport nonegotiate

S2(config)#interface Fa0/1
S2(config-if)#switchport nonegotiate
{% endhighlight bash %}

#### Step 4: Verify the trunking configuration on S1 and S2.

{% highlight bash %}
S1#show interface fa0/1 trunk
S2#show interface fa0/1 trunk
{% endhighlight bash %}

#### Step 5: Enable storm control for broadcasts on S1 and S2 trunk ports.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#storm-control broadcast level 50

S2(config)#interface FastEthernet 0/1
S2(config-if)#storm-control broadcast level 50
{% endhighlight bash %}

#### Step 6: Disable trunking on S1 access ports that are in use.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#switchport mode access

S1(config)#interface FastEthernet 0/6
S1(config-if)#switchport mode access
{% endhighlight bash %}

#### Step 7: Enable PortFast on S1 access ports that are in use.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#spanning-tree portfast

S1(config)#interface FastEthernet 0/6
S1(config-if)#spanning-tree portfast
{% endhighlight bash %}

#### Step 8: Enable BPDU guard on S1 access ports that are in use.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#spanning-tree bpduguard enable

S1(config)#interface FastEthernet 0/6
S1(config-if)#spanning-tree bpduguard enable
{% endhighlight bash %}

### **Task 3: Configure Port Security and Disable Unused Ports.**

#### Step 1: Configure basic port security for the S1 access port.

> Use the default port security options (set maximum MAC addresses to 1 and violation action to shutdown). Allow the secure MAC address that is dynamically learned on a port be added to the switch running configuration.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#shutdown
S1(config-if)#switchport port-security
S1(config-if)#switchport port-security mac-address [your switch mac address]
S1(config-if)#switchport port-security mac-address sticky
S1(config-if)#no shutdown

S1(config)#interface FastEthernet 0/6
S1(config-if)#shutdown
S1(config-if)#switchport port-security
S1(config-if)#switchport port-security mac-address [your switch mac address]
S1(config-if)#switchport port-security mac-address sticky
S1(config-if)#no shutdown
{% endhighlight bash %}

#### Step 2: Disable unused ports on S1.

> As a further security measure, disable any ports not being used.

{% highlight bash %}
S1(config)#interface range FastEthernet 0/2-5
S1(config)#shutdown
S1(config)#interface range FastEthernet 0/7-24
S1(config)#shutdown
{% endhighlight bash %}

---

## <center>6. Configure ASA Basic Settings and Firewall</center>

---

### **Task 1: Prepare the ASA for ASDM access.**

#### Step 1: Clear the previous ASA configuration settings.

{% highlight bash %}
ciscoasa# write erase
ciscoasa# show start
ciscoasa# reload
{% endhighlight bash %}

#### Step 2: Bypass Setup Mode and configure the VLAN/routed interfaces using CLI.

> The VLAN 1 logical interface will be used by PC-B to access ASDM on ASA physical interface E0/1. Configure interface VLAN 1 and name it “inside”. Specify IP address 192.168.10.1 and subnet mask 255.255.255.0. Verify that the security level is set to 100.

{% highlight bash %}
ciscoasa(config)# interface vlan 1
ciscoasa(config-if)# nameif inside

“INFO: Security level for "inside" set to 100 by default.”

ciscoasa(config-if)# ip address 192.168.10.1 255.255.255.0
ciscoasa(config-if)# exit
{% endhighlight bash %}

> Pre-configure interface VLAN 2 and name it “outside”, and add physical interface E0/0 to VLAN 2. You will assign the IP address using ASDM. Verify that the security level is set to 0.

{% highlight bash %}
ciscoasa(config)# interface vlan 2
ciscoasa(config-if)# nameif outside

“INFO: Security level for "outside" set to 0 by default.”

ciscoasa(config-if)# interface e0/0
ciscoasa(config-if)# switchport access vlan 2
ciscoasa(config-if)# no shut
ciscoasa(config-if)# exit
{% endhighlight bash %}

> Test Connectivity to the ASA by pinging from PC-B to ASA interface VLAN 1 IP address 192.168.10.1. The pings should be successful.

{% highlight bash %}
On PC-B >> ping 192.168.10.1
{% endhighlight bash %}

#### Step 3: Configure and verify access to the ASA from the inside network.

> Configure the ASA to accept HTTPS connections and to allow access to ASDM from any host on the inside network 192.168.10.0/24.

{% highlight bash %}
ciscoasa(config)# http server enable
ciscoasa(config)# http 192.168.10.0 255.255.255.0 inside
{% endhighlight bash %}

> Open a browser on PC-B and test the HTTPS access to the ASA ASDM GUI.

{% highlight bash %}
On PC-B >> https://192.168.10.1
{% endhighlight bash %}

### **Task 2: Configure basic ASA settings using the ASDM Startup Wizard.**

#### Step 1: Access the Configuration menu and launch the Startup wizard.

{% highlight bash %}
Click the Configuration button at the top left of the screen. There are five main configuration areas:

• Device Setup
• Firewall
• Remote Access VPN
• Site-to-Site VPN
• Device Management
{% endhighlight bash %}

#### Step 2: Configure hostname, domain name, and enable password.

> Configure the ASA host name CCNAS-ASA and domain name of ccnasecurity.com. Change the enable mode password to ciscoenapa55.

{% highlight bash %}
• On first startup wizard >> ensure “Modify Existing Configuration” option is selected.
• On step 2 wizard screen, enter the following;

Hostname: CCNAS-ASA
Domain name: ccnasecurity.com
Password: ciscoenapa55 << You must click the checkbox for changing the enable mode password and change it from blank (no password) to ciscoenapa55
{% endhighlight bash %}

#### Step 3: Configure the outside VLAN interface.

> Enter an outside IP address of 209.165.200.234 and mask 255.255.255.248.

{% highlight bash %}
• On step 3 wizard screen >> for Outside and Inside VLANs, do not change the current settings. For DMZ VLAN, select “Do not configure” button and uncheck “Enable VLAN”
• On step 4 wizard screen >> verify that port Ethernet1 is in Inside VLAN 1 and that port Ethernet0 is in Outside VLAN 2.
• On step 5 wizard screen >> Interface IP Address Configuration, enter the following outside IP address:

IP address: 209.165.200.234
Mask: 255.255.255.248
{% endhighlight bash %}

#### Step 4: Configure DHCP, address translation and administrative access.

> Enable the DHCP server on the Inside Interface and specify a starting IP address of 192.168.10.5 and ending IP address of 192.168.10.30. Enter the DNS server 1 address of 10.3.3.3 and domain name ccnasecurity.com.

{% highlight bash %}
• On step 6 wizard screen >> select checkbox “Enable DHCP server on the inside interface”. Enter the following;

Starting IP address: 192.168.10.5
Ending IP address: 192.168.10.30
DNS Server 1: 10.3.3.3
Domain Name: ccnasecurity.com
{% endhighlight bash %}

> Configure the ASA to use port address translation (PAT) using the IP address of the outside interface.

{% highlight bash %}
• On step 7 wizard screen >> Ensure “Use Port Address Translation (PAT)” and “Use the IP address on the outside interface” is selected only.
{% endhighlight bash %}

> Add Telnet access to the ASA for the inside network 192.168.10.0 with a subnet mask of 255.255.255.0. Add SSH access to the ASA from host 172.30.3.3 on the outside network.

{% highlight bash %}
• On step 8 wizard screen >> Add the following entries >> Click Add.

Type: Telnet
Interface: inside
IP address: 192.168.10.0
Mask: 255.255.255.0

Type: SSH
Interface: outside
IP address: 172.30.3.3
Mask: 255.255.255.255

• Ensure that “Enable HTTP server for HTTPS/ASDM access” is checked.
• On step 9 wizard – Startup Wizard Summary >> review the settings, click Finish.
• Restart ASDM and provide the new enable password ciscoenapa55 with no username. Return to the Device Dashboard and check the Interface Status window.
{% endhighlight bash %}

#### Step 5: Test Telnet access to the ASA.

> From a command prompt or GUI Telnet client on PC-B, Telnet to the ASA inside interface at IP address 192.168.10.1.

{% highlight bash %}
>> telnet 192.168.10.1
{% endhighlight bash %}

### **Task 3: Configuring ASA Settings from the ASDM Configuration Menu.**

#### Step 1: Set the ASA Date and Time.

> Set the time zone, current date and time and apply the commands to the ASA.

{% highlight bash %}
• Configuration screen > Device Setup menu > System Time > Clock
• Select your Time Zone from drop-down menu > enter current date and time in the fields provided.
• Click Apply to send the commands to the ASA.
{% endhighlight bash %}

#### Step 2: Configure a static default route for the ASA.

{% highlight bash %}
• From the ASDM Tools menu, select Ping and enter the IP address of router R1 S0/0/0 (10.10.10.1).

>> ASA does not have a default route to unknown external networks. The ping should fail because the ASA has no route to 10.10.10.1. Click Close to continue.

• Configuration screen > Device Setup menu > Routing > Static Routes. Click “IPv4 Only” button and click Add to add a new static route.
• In the Add Static Route dialogue box, choose the outside interface from the drop down menu.
• Click on the ellipsis button (. .) next to Network.
• Select Any from the list of network objects, then click OK.
• The selection of “Any” translates to a “quad zero” route. For the Gateway IP, enter 209.165.200.233
• Click OK and click Apply to send the commands to the ASA.
{% endhighlight bash %}

> Require authentication for HTTP/ASDM, SSH and Telnet connections and specify the “LOCAL” server group for each connection type.

{% highlight bash %}
• Configuration screen > Device Management area > click Users/AAA
• Click AAA Access.
• On the Authentication tab, click the checkbox to require authentication for HTTP/ASDM, SSH and Telnet connections
• Specify the “LOCAL” server group for each enabled connection type.
• Click Apply to send the commands to the ASA.
{% endhighlight bash %}

> From PC-C, open an SSH client such as PuTTY and attempt to access the ASA outside interface at 209.165.200.234. You should be able to establish the connection.

{% highlight bash %}
• Open PuTTY on PC-C > select the SSH option. Ensure the port number is 22.
• Enter the IP address: 209.165.200.234
{% endhighlight bash %}

---

## <center>7. Configure ASA AnyConnect SSL VPN Remote Access</center>

---

#### Step 1: Configure the SSL VPN user interface.

> Configure VPN-Con-Prof as the Connection Profile Name, and specify outside as the interface to which outside users will connect.

{% highlight bash %}
• On PC-B open up browser > type in: https://192.168.10.1
• A security warning will appear about the website security certificate. Click Continue to this website.
• Click Yes for any other security warnings.
• At the ASDM welcome page, click the Run ASDM button.
• Login as user admin with password cisco123.
• From the ASDM main menu, select Wizards > VPN Wizards > Clientless SSL VPN wizard.
• Click Next.
• On the SSL VPN Interface screen (Step 2 of 6), configure “ClientlessVPN-Con-Prof” as the Connection Profile Name, and specify outside as the interface to which outside users will connect. Click Next.
{% endhighlight bash %}

#### Step 2: Configure AAA user authentication.

> Use the local user database to authenticate remote access users and create a new user named VPNuser with a password of remote.

{% highlight bash %}
• On the User Authentication screen (Step 3 of 6) click Authenticate using the local user database button.
• Enter the username VPNuser and the password remote. Click Add to create the new user. Click Next to continue.
{% endhighlight bash %}

#### Step 3: Configure the VPN group policy.

> Create a new group policy named VPN-Grp-Pol.

{% highlight bash %}
• On the Group Policy screen (Step 4 of 6) create a new group policy named VPN-Grp-Pol. Click Next to continue.
{% endhighlight bash %}

#### Step 4: Configure the bookmark list.

> Add a bookmark list and name it WebServer-XX (where XX is your initials).

{% highlight bash %}
• From the Clientless Connections Only – Bookmark List screen (Step 5 of 6) click the Manage button to create an HTTP server bookmark.
• In the Configure GUI Customization Objects window, click Add to open the Add Bookmark List window.
• Name the list Web-Server-XX.

>> FROM HERE DO NOT CLICK OK YET!
{% endhighlight bash %}

> Add a new Bookmark with Web Mail as the Bookmark Title. Specify the server destination IP address of PC-B 192.168.10.3 (simulating a web server).

{% highlight bash %}
• From the Add Bookmark List window, click Add to open the Add Bookmark window.
• Enter Web Mail as the Bookmark Title.
• Enter the server destination IP address 192.168.10.3 as the URL.
• Click OK in the Add Bookmark window to return to the Configure GUI Customization Objects window.
• Select the desired bookmark and click OK to return to the Bookmark List window. Click Next to continue.
• The Summary page (Step 6 of 6) is displayed next. Verify that the information configured in the SSL VPN wizard is correct.
• Click Finish to complete the process and deliver the commands to the ASA.

To verify the connection profile;

• Configuration > Remote Access VPN > Clientless SSL VPN Access > Connection Profiles. From this window the VPN configuration can be verified and edited.
{% endhighlight bash %}

#### Step 5: Verify VPN access from the remote host.

> Open the browser on PC-C and enter the login URL for the SSL VPN into the address field (https://209.165.200.234). The Logon window should appear. Enter the previously configured user name VPNuser and password remote and click Logon to continue. The Web Portal window should display.

{% highlight bash %}
• On PC-C open up browser >> in the address bar, type in: https://209.165.200.234
• Enter the following credentials;

Username: VPNuser
Password: remote

• Click Logon to continue. Now the web portal should display!
{% endhighlight bash %}