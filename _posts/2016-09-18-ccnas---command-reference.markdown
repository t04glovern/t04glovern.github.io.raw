---
layout: post
title: CCNAS - Command Reference
date: 2016-09-18
comments: true
description: I analyse a number of command line tools required for CCNAS Hand on exam
share: true
author: nathan
tags:
 - ccnas
---

## <center>Contents</center>

---

1. Hostname
2. Interface - IP Address
3. Interface - Clock Rate
4. DNS Lookup Disable
5. Static default route
6. Static route to simulated LAN (Loopback 1)
7. VLAN Configuration
8. Password Length
9. Enable secret password
10. Encrypt plaintext passwords
11. Configure console lines
12. Warning message / Banner
13. Configure VTY lines
14. Enable HTTP access on router
15. Configure the local user database
16. Enable AAA services
17. Implement AAA services using RADIUS/TACACS+/local database
18. Configure domain name
19. Configure the incoming VTY lines
20. Generate the RSA encryption key pair
21. Configure the SSH version
22. Secure against login attacks
23. Configure the site-to-site VPN
24. Generate a mirror configuration for the other router
25. View security association
26. Configure a ZPF Firewall
27. Configure IPS on R3 Using CCP
28. Configure Trunk Ports
29. Change the native VLAN
30. Prevent the use of DTP
31. Enable storm control for broadcasts
32. Disable trunking
33. Enable PortFast on access ports that are in use
34. Enable BPDU guard
35. Configure basic port security
36. Disable unused ports
37. Configure ASA basic settings and firewall
38. Configure basic ASA settings using the ASDM Startup Wizard
39. Set the ASA Date and Time with ASDM
40. Configure ASA AnyConnect SSL VPN Remote Access

---

## <center>Reference Topology</center>

---

![Topology]({{ site.url }}/images/posts/2016.09.18/ccnas-practice-skills-topology.png){:.center-image}

---

## <center>Reference Addressing Table</center>

---

![Topology]({{ site.url }}/images/posts/2016.09.18/ccnas-practice-skills-addressing-table.png){:.center-image}

---

## <center>Command References</center>

---

### **1. Hostname**

{% highlight bash %}
Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#R1
{% endhighlight bash %}

### **2. Interface - IP Address**

{% highlight bash %}
R1(config)#interface S0/0/0
R1(config-if)#ip address 10.10.10.1 255.255.255.252
{% endhighlight bash %}

### **3. Interface - Clock Rate**

Clock rate is applied to the DCE side of the serial connection

{% highlight bash %}
R1(config)#interface S0/0/0
R1(config-if)#clock rate 64000
{% endhighlight bash %}

### **4. DNS Lookup Disable**

{% highlight bash %}
R1(config)#no ip domain-lookup
{% endhighlight bash %}

### **5. Static default route**

Static default route from R1 to R2

![Topology]({{ site.url }}/images/posts/2016.09.18/static-default-route.png){:.center-image}

{% highlight bash %}
R1(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.2
{% endhighlight bash %}

### **6. Static route to simulated LAN (Loopback 1)**

Static routes from R2 to the R1 simulated LAN (Loopback 1)

![Topology]({{ site.url }}/images/posts/2016.09.18/static-route-loopback.png){:.center-image}

{% highlight bash %}
R2(config)#ip route 10.10.10.2 255.255.255.0 172.20.1.1
{% endhighlight bash %}

### **7. VLAN Configuration**

Interface details

![Topology]({{ site.url }}/images/posts/2016.09.18/vlan-configuration-specs.png){:.center-image}

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

### **8. Password Length**

Configure a minimum password length of 10 characters on R1.

{% highlight bash %}
R1(config)#security passwords min-length 10
{% endhighlight bash %}

### **9. Enable secret password**

Use an enable secret password of ciscoenapa55.

{% highlight bash %}
R1(config)#enable secret ciscoenapa55
{% endhighlight bash %}

### **10. Encrypt plaintext passwords**

{% highlight bash %}
R1(config)#service password-encryption
{% endhighlight bash %}

### **11. Configure console lines**

- Configure a console password of ciscoconpa55 and enable login
- Set the exec-timeout to log out after 15 minutes of inactivity
- Prevent console messages from interrupting command entry

{% highlight bash %}
R1(config)#line console 0
R1(config-line)#password ciscoconpa55
R1(config-line)#exec-timeout 15 0
R1(config-line)#login
R1(config-line)#logging synchronous
{% endhighlight bash %}

### **12. Warning message / Banner**

{% highlight bash %}
R1(config)#banner motd $Unauthorized access strictly prohibited and prosecuted to the full extent of the law!$
{% endhighlight bash %}

### **13. Configure VTY lines**

- Configure the password for vty lines to be ciscovtypa55 and enable login
- Set the exec-timeout so a session is logged out after 15 minutes of inactivity

{% highlight bash %}
R2(config)#line vty 0 4
R2(config-line)#password ciscovtypa55
R2(config-line)#exec-timeout 15 0
R2(config-line)#login
{% endhighlight bash %}

### **14. Enable HTTP access on router**

{% highlight bash %}
R2(config)#ip http server
{% endhighlight bash %}

### **15. Configure the local user database**

- Create a local user account of Admin01 with a secret password of Admin01pa55
- privilege level of 15

{% highlight bash %}
R1(config)#username Admin01 privilege 15 secret Admin01pa55
{% endhighlight bash %}

### **16. Enable AAA services**

{% highlight bash %}
R1(config)#aaa new-model
{% endhighlight bash %}

### **17. Implement AAA services using RADIUS/TACACS+/local database**

- Create the default login authentication method list using RADIUS as the first option
- TACACS+ as the second option
- case-sensitive local authentication as the third option
- enable password as the backup option to use if an error occurs in relation to local authentication

{% highlight bash %}
R1(config)#aaa authentication login default group radius enable
R1(config)#aaa authentication login default group tacacs+ enable
R1(config)#aaa authentication login default local enable
{% endhighlight bash %}

### **18. Configure domain name**

{% highlight bash %}
R1(config)#ip domain-name ccnasecurity.com
{% endhighlight bash %}

### **19. Configure the incoming vty lines**

- Specify that the router vty lines will accept only SSH connections

{% highlight bash %}
R1(config)#line vty 0 4
R1(config-line)#transport input ssh
R1(config-line)#exit
{% endhighlight bash %}

### **20. Generate the RSA encryption key pair**

- Configure the RSA keys with 1024 as the number of modulus bits

{% highlight bash %}
R1(config)#crypto key generate rsa general-keys modulus 1024
{% endhighlight bash %}

### **21. Configure the SSH version**

- Specify that the router accept only SSH version 2 connections

{% highlight bash %}
R1#show ip ssh
{% endhighlight bash %}

### **22. Secure against login attacks**

{% highlight bash %}
R1(config)#login on-failure log
{% endhighlight bash %}

### **23. Configure the site-to-site VPN**

![Topology]({{ site.url }}/images/posts/2016.09.18/site-to-site-vpn-ipsec-setup.png){:.center-image}

- From the CLI, configure an enable secret password of ciscoenapa55 for use with CCP on R3.

{% highlight bash %}
R3(config)#enable secret ciscoenapa55
{% endhighlight bash %}

- Enable the HTTP server on R3.

{% highlight bash %}
R3(config)# ip http server
{% endhighlight bash %}

- Add user Admin01 to the local database with a privileged level of 15, and a password of Admin01pa55.

{% highlight bash %}
R3(config)# username Admin01 privilege 15 secret Admin01pa55
{% endhighlight bash %}

- Configure local database authentication of HTTP sessions.

{% highlight bash %}
R3(config)# ip http authentication local
{% endhighlight bash %}

- From PC-C run CCP and access R3.
- Manage Devices window > R3 IP address 172.30.3.1 in the first IP address field.
- Enter Admin01 in the Username field, and Admin01pa55 in the Password field.
- At the CCP Dashboard, click the Discover button to discover and connect to R3.

![Topology]({{ site.url }}/images/posts/2016.09.18/ccp-manage-devices.png){:.center-image}

- Click the Configure button at the top of the CCP screen.
- Choose Security > VPN > Site-to-Site VPN.
- Click the Launch the selected task button to begin the CCP Site-to-Site VPN wizard.
- From the initial Site-to-Site VPN wizard screen, choose the Step by step wizard, and then click Next.

![Topology]({{ site.url }}/images/posts/2016.09.18/ccp-vpn-setup-start.png){:.center-image}

- On the VPN Connection Information screen, select the interface for the connection, which should be R3 Serial0/0/1.
- In the Peer Identity section, select Peer with static IP address and enter the IP address of the remote peer, R1 interface S0/0/0, which is 10.10.10.1.

![Topology]({{ site.url }}/images/posts/2016.09.18/ccp-vpn-connection-settings.png){:.center-image}

- Specify the pre-shared VPN key cisco12345.
- Encrypt traffic between the R3 LAN and the R1 Loopback 1 simulated LAN.
- On the IKE Proposals screen, click Next to continue.
- On the Transform Set screen, click Next to continue.
- On the Traffic to protect screen, enter the following information:

![Topology]({{ site.url }}/images/posts/2016.09.18/ccp-vpn-transform-set.png){:.center-image}

- Local Network (R3 LAN) : IP address: 172.30.3.1 Subnet Mask: 255.255.255.0
- Remote Network (R1 Loopback) : IP address: 172.20.1.1 Subnet Mask: 255.255.255.0

### **24. Generate a mirror configuration for the other router**

![Topology]({{ site.url }}/images/posts/2016.09.18/ccp-vpn-export-config.png){:.center-image}

- Click the Configure button at the top of the CCP screen.
- Choose Security > VPN > Site-to-Site VPN. Click the Edit Site to Site VPN tab.
- Select the VPN policy you just configured on R1 and click the Generate Mirror button in the lower right of the window.

The Generate Mirror window displays the commands necessary to configure R3 as a VPN peer. Scroll through the window to see all the commands generated.

- Click the Save button to create a text file. Name it VPN-Mirror-Cfg-for-R3.txt.
- Apply the crypto map to the R1 VPN interface.

- On R1, enter privileged EXEC mode and then global config mode.
- Copy the commands from the text file into the R1 CLI.

- To apply the crypto map to R1 VPN interface, enter the following:

{% highlight bash %}
R1(config)#interface S0/0/0
R1(config-if)#crypto map SDM_CMAP_1
{% endhighlight bash %}

### **25. View security association**

- Issue the show crypto isakmp sa command on R3 to view the security association created.

{% highlight bash %}
R3#show crypto isakmp sa
{% endhighlight bash %}

- Issue the show crypto ipsec sa command on R1 to verify packets are being received from R3 and decrypted by R1.

{% highlight bash %}
R1#show crypto isakmp sa
{% endhighlight bash %}

### **26. Configure a ZPF Firewall**

- Configure a Basic firewall with Fa0/1 interface as the Inside interface and S0/0/1 as the Outside interface.
- Configure > Security > Firewall > Firewall. Select Basic Firewall.
- Click the Launch the selected task button. Click Next to continue.
- Check the Inside (trusted) check box for Fast Ethernet0/1 and the Outside (untrusted) check box for Serial0/0/1. Click Next.
- Click OK when the warning is displayed informing you that you cannot launch CCP from the S0/0/1 interface after the Firewall wizard completes.
- Use the Low Security setting, and complete the Firewall wizard.
- Move the slider to Low Security and click the Preview Commands button to preview the commands that are delivered to the router. Click Next to continue.
- On the Review the Firewall Configuration Summary screen, click Finish to complete the Firewall wizard.

### **27. Configure IPS on R3 Using CCP**

- Verify or create the IPS directory, ipsdir, in router flash on R3.

{% highlight bash %}
R3#mkdir ipsdir
R3#dir flash:
{% endhighlight bash %}

- Launch the IPS wizard and apply the IPS rule in the inbound direction for Serial0/0/1.
- Click the Configure button at the top of the CCP screen.
- Choose Security > Intrusion Prevention > Create IPS.
- Click Launch IPS Rule Wizard. Click Next to continue.
- In the Select Interfaces window, check the Inbound check box for Fast Ethernet0/1 and Serial0/0/1. Click Next.
- Specify the signature file with a URL and use TFTP to retrieve the file from PC-C.
- Signature File and Public Key window, click the ellipsis (...) button next to Specify the Signature File You Want to Use with IOS IPS to open the Specify Signature File window. Confirm that the Specify signature file using URL option is chosen.
- For Protocol, select tftp from the drop-down menu.
- Enter the IP address of the PC-C TFTP server and the filename. The address is 172.30.3.3/IOS-Sxxx-CLI.pkg (where xxx is the number of the package)
- Click OK to return to the Signature File and Public Key window.
Name the public key file realm-cisco.pub.
- In the Configure Public Key section of the Signature File and Public Key window, enter realm-cisco.pub in the Name field.
- Copy the text from the public key file to the CCP IPS wizard.
- Open the realm-cisco-pub-key.txt file located on PC-C.
- Copy the text between the phrase key-string and the word quit into the Key field in the Configure Public Key section.
- Click Next to display the Config Location and Category window.
Specify the flash:/ipsdir/ directory name as the location to store the signature information.
- In the Config Location and Category window in the Config Location section, click the ellipsis (...) button next to Config Location to add the location.
- Verify that Specify the config location on this router is selected. Click the ellipsis (...) button.
- Click the plus sign (+) next to flash. Choose ipsdir and then click OK.
- Choose the basic category.
- In the Choose Category field of the Config Location and Category window, choose basic.
- Complete the wizard.
- Click Next in the Cisco CCP IPS Policies Wizard window.
- Click Finish in the IPS Policies Wizard window and review the commands that will be delivered to the router.
- Click Deliver.
- Click OK when the Commands Deliver Status window is ready.
- When the signature configuration process has completed, you return to the IPS window with the Edit IPS tab selected.

### **28. Configure Trunk Ports**

- Configure trunk ports on S1 and S2.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#switchport mode trunk
S2(config)#interface FastEthernet 0/1
S2(config-if)#switchport mode trunk
{% endhighlight bash %}

### **29. Change the native VLAN**

- Change the native VLAN to 99 for the trunk ports on S1 and S2.

{% highlight bash %}
S1(config)#interface Fa0/1
S1(config-if)#switchport trunk native vlan 99
S1(config-if)#end
S2(config)#interface Fa0/1
S2(config-if)#switchport trunk native vlan 99
S2(config-if)#end
{% endhighlight bash %}

### **30. Prevent the use of DTP**

- Prevent the use of DTP on S1 and S2 trunk ports.

{% highlight bash %}
S1(config)#interface Fa0/1
S1(config-if)#switchport nonegotiate

S2(config)#interface Fa0/1
S2(config-if)#switchport nonegotiate
{% endhighlight bash %}

### **31. Enable storm control for broadcasts**

- Enable storm control for broadcasts on S1 and S2 trunk ports.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#storm-control broadcast level 50

S2(config)#interface FastEthernet 0/1
S2(config-if)#storm-control broadcast level 50
{% endhighlight bash %}

### **32. Disable trunking**

- Disable trunking on S1 access ports that are in use.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#switchport mode access

S1(config)#interface FastEthernet 0/6
S1(config-if)#switchport mode access
{% endhighlight bash %}

### **33. Enable PortFast on access ports that are in use**

- Enable PortFast on S1 access ports that are in use.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#spanning-tree portfast

S1(config)#interface FastEthernet 0/6
S1(config-if)#spanning-tree portfast
{% endhighlight bash %}

### **34. Enable BPDU guard**

- Enable BPDU guard on S1 access ports that are in use.

{% highlight bash %}
S1(config)#interface FastEthernet 0/1
S1(config-if)#spanning-tree bpduguard enable

S1(config)#interface FastEthernet 0/6
S1(config-if)#spanning-tree bpduguard enable
{% endhighlight bash %}

### **35. Configure basic port security**

- Use the default port security options (set maximum MAC addresses to 1 and violation action to shutdown).
- Allow the secure MAC address that is dynamically learned on a port be added to the switch running configuration.

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

### **36. Disable unused ports**

- Disable unused ports on S1.

{% highlight bash %}
S1(config)#interface range FastEthernet 0/2-5
S1(config)#shutdown
S1(config)#interface range FastEthernet 0/7-24
S1(config)#shutdown
{% endhighlight bash %}

### **37. Configure ASA basic settings and firewall**

- Clear the previous ASA configuration settings.

{% highlight bash %}
ciscoasa# write erase
ciscoasa# show start
ciscoasa# reload
{% endhighlight bash %}

- Bypass Setup Mode and configure the VLAN/routed interfaces using CLI.
- The VLAN 1 logical interface will be used by PC-B to access ASDM on ASA physical interface E0/1. 
- Configure interface VLAN 1 and name it “inside”.
- Specify IP address 192.168.10.1 and subnet mask 255.255.255.0.
- Verify that the security level is set to 100.

{% highlight bash %}
ciscoasa(config)# interface vlan 1
ciscoasa(config-if)# nameif inside

“INFO: Security level for "inside" set to 100 by default.”

ciscoasa(config-if)# ip address 192.168.10.1 255.255.255.0
ciscoasa(config-if)# exit
{% endhighlight bash %}

- Pre-configure interface VLAN 2 and name it “outside”, and add physical interface E0/0 to VLAN 2.
- You will assign the IP address using ASDM. Verify that the security level is set to 0.

{% highlight bash %}
ciscoasa(config)# interface vlan 2
ciscoasa(config-if)# nameif outside

“INFO: Security level for "outside" set to 0 by default.”

ciscoasa(config-if)# interface e0/0
ciscoasa(config-if)# switchport access vlan 2
ciscoasa(config-if)# no shut
ciscoasa(config-if)# exit
{% endhighlight bash %}

- Configure and verify access to the ASA from the inside network.
- Configure the ASA to accept HTTPS connections and to allow access to ASDM from any host on the inside network 192.168.10.0/24.

{% highlight bash %}
ciscoasa(config)# http server enable
ciscoasa(config)# http 192.168.10.0 255.255.255.0 inside
{% endhighlight bash %}

### **38. Configure basic ASA settings using the ASDM Startup Wizard**

- Access the Configuration menu and launch the Startup wizard.

{% highlight bash %}
Click the Configuration button at the top left of the screen. There are five main configuration areas:

• Device Setup
• Firewall
• Remote Access VPN
• Site-to-Site VPN
• Device Management
{% endhighlight bash %}

- Configure hostname, domain name, and enable password.
- Configure the ASA host name CCNAS-ASA and domain name of ccnasecurity.com. 
- Change the enable mode password to ciscoenapa55.

{% highlight bash %}
• On first startup wizard >> ensure “Modify Existing Configuration” option is selected.
• On step 2 wizard screen, enter the following;

Hostname: CCNAS-ASA
Domain name: ccnasecurity.com
Password: ciscoenapa55 << You must click the checkbox for changing the enable mode password and change it from blank (no password) to ciscoenapa55
{% endhighlight bash %}

- Configure the outside VLAN interface.
- Enter an outside IP address of 209.165.200.234 and mask 255.255.255.248.

{% highlight bash %}
• On step 3 wizard screen >> for Outside and Inside VLANs, do not change the current settings. For DMZ VLAN, select “Do not configure” button and uncheck “Enable VLAN”
• On step 4 wizard screen >> verify that port Ethernet1 is in Inside VLAN 1 and that port Ethernet0 is in Outside VLAN 2.
• On step 5 wizard screen >> Interface IP Address Configuration, enter the following outside IP address:

IP address: 209.165.200.234
Mask: 255.255.255.248
{% endhighlight bash %}

- Configure DHCP, address translation and administrative access.
- Enable the DHCP server on the Inside Interface and specify a starting IP address of 192.168.10.5 and ending IP address of 192.168.10.30.
- Enter the DNS server 1 address of 10.3.3.3 and domain name ccnasecurity.com.

{% highlight bash %}
• On step 6 wizard screen >> select checkbox “Enable DHCP server on the inside interface”. Enter the following;

Starting IP address: 192.168.10.5
Ending IP address: 192.168.10.30
DNS Server 1: 10.3.3.3
Domain Name: ccnasecurity.com
{% endhighlight bash %}

- Configure the ASA to use port address translation (PAT) using the IP address of the outside interface.

{% highlight bash %}
• On step 7 wizard screen >> Ensure “Use Port Address Translation (PAT)” and “Use the IP address on the outside interface” is selected only.
{% endhighlight bash %}

- Add Telnet access to the ASA for the inside network 192.168.10.0 with a subnet mask of 255.255.255.0.
- Add SSH access to the ASA from host 172.30.3.3 on the outside network.

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

### **39. Set the ASA Date and Time with ASDM**

- Set the time zone, current date and time and apply the commands to the ASA.

{% highlight bash %}
• Configuration screen > Device Setup menu > System Time > Clock
• Select your Time Zone from drop-down menu > enter current date and time in the fields provided.
• Click Apply to send the commands to the ASA.
{% endhighlight bash %}

- Configure a static default route for the ASA.

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

- Require authentication for HTTP/ASDM, SSH and Telnet connections and specify the “LOCAL” server group for each connection type.

{% highlight bash %}
• Configuration screen > Device Management area > click Users/AAA
• Click AAA Access.
• On the Authentication tab, click the checkbox to require authentication for HTTP/ASDM, SSH and Telnet connections
• Specify the “LOCAL” server group for each enabled connection type.
• Click Apply to send the commands to the ASA.
{% endhighlight bash %}

- From PC-C, open an SSH client such as PuTTY and attempt to access the ASA outside interface at 209.165.200.234.

{% highlight bash %}
• Open PuTTY on PC-C > select the SSH option. Ensure the port number is 22.
• Enter the IP address: 209.165.200.234
{% endhighlight bash %}

### **40. Configure ASA AnyConnect SSL VPN Remote Access**

- Configure the SSL VPN user interface.
- Configure VPN-Con-Prof as the Connection Profile Name, and specify outside as the interface to which outside users will connect.

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

- Configure AAA user authentication.
- Use the local user database to authenticate remote access users and create a new user named VPNuser with a password of remote.

{% highlight bash %}
• On the User Authentication screen (Step 3 of 6) click Authenticate using the local user database button.
• Enter the username VPNuser and the password remote.
• Click Add to create the new user.
• Click Next to continue.
{% endhighlight bash %}

- Configure the VPN group policy.
- Create a new group policy named VPN-Grp-Pol.

{% highlight bash %}
• On the Group Policy screen (Step 4 of 6) create a new group policy named VPN-Grp-Pol.
• Click Next to continue.
{% endhighlight bash %}

- Configure the bookmark list.
- Add a bookmark list and name it WebServer-XX (where XX is your initials).

{% highlight bash %}
• From the Clientless Connections Only – Bookmark List screen (Step 5 of 6) click the Manage button to create an HTTP server bookmark.
• In the Configure GUI Customization Objects window, click Add to open the Add Bookmark List window.
• Name the list Web-Server-XX.

>> FROM HERE DO NOT CLICK OK YET!
{% endhighlight bash %}

- Add a new Bookmark with Web Mail as the Bookmark Title.
- Specify the server destination IP address of PC-B 192.168.10.3 (simulating a web server).

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

- Verify VPN access from the remote host.
- Open the browser on PC-C and enter the login URL for the SSL VPN into the address field (https://209.165.200.234).
- The Logon window should appear.
- Enter the previously configured user name VPNuser and password remote and click Logon to continue.
- The Web Portal window should display.

{% highlight bash %}
• On PC-C open up browser >> in the address bar, type in: https://209.165.200.234
• Enter the following credentials;

Username: VPNuser
Password: remote

• Click Logon to continue. Now the web portal should display!
{% endhighlight bash %}