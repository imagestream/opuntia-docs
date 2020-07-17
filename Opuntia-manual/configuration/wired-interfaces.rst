=====================================
Wired Ethernet Interface Configuraion
=====================================

.. contents:: Table of Contents

Interfaces in Opuntia
---------------------

A familiar concept in computer networking is that of the "Interface". An "Interface" is a software construct that allows 
computer systems to interact with networks. An interface can represent a physical device like an Ethernet port. It can also 
represent virtual devices like a Loopback device. 

Opuntia extends this concept slightly. Opuntia is based on a Linux operating system and the base Linux system can have many 
different network Interfaces. An Opuntia interface can be mapped to a Linux Interface. And The Opuntia Interface can also 
be a group of bridged Linux Interfaces. By bridging multiple interfaces together they all share the same network IP / IPv6
address space. And any host connected to any of these interfaces can communicate directly.   

For example a common configuration is to have a *Lan* and *Wan* network interface. The *Wan* interface is mapped to an 
Linux interface that is connected to an upstream Internet provider. The *Lan* interface is often a brige of all of the other
Ethernet interfaces on the router. This replicates a common CPE (Customer premises equipment) configuration where one
interface is used to connect to an Internet provider and all other interfaces in the system share a network. 

This relationship between an interface in Opuntia and the base Linux networking interface is a key concept that is also
needed to understand how to configure VLAN tags on Ethernet devices. This will be covered in detail in the VLAN section. 
But as a brief introduction it is the mapping of the Opuntia interface to the Linux networking interface that controls
if traffic leaving the system will be VLAN tagged. 

So Opuntia networking interfaces are either a 1:1, 1:many or even a many:1 mapping with respect to the Linux networking 
interfaces. The Opuntia interfaces have flexible naming making it easy to understand the purpose of an interface; for example
name of *Wan* vs a Linux device name of *enp97s0f0*. They are also used to define membership of a firewall zone. 

In Opuntia each interface has an associated network protocol. Going back to the common example of CPE device. 
The *Wan* interface normally configured with protocol "DHCP Client". This configuration results in the system making an IPv4 
DHCP request on the physical interface connected to the ISP. If IPv6 DHCP is required it's common to have a *Wan_6* interface also 
configured. This Opuntia interface shares the same physical interface with the *Wan* interface. But it has a different
network protocol configuration in this case "DHCPv6 client". The *Lan* interface would have a "Static address" configured.
This static address will often be a private `RFC 1918 address <https://tools.ietf.org/html/rfc1918>`_. 

Below is the Network Interface page from a router configured in this scenario. You will see a pair of *Wan* and *Wan_6* 
interfaces. These interfaces have protocol "DHCP" and "DHCPv6" configured. In this we are getting ip address 
192.168.79.14/24 on the *Wan* interface and 2605:6000:62e0:400::1fa/128 on the *Wan_6* interface. Both the *Wan* and *Wan_6* 
interface are sharing the same Linux device in this case *eth0*.  The *Lan* interface is configured with a static IPv4 address
of 10.10.199.1/24 and the IPv6 subsystem has assigned a network range of 2605:6000:62e0:410::1/62 to this interface. 
The color code of the devices tell you which firewall zone applies to the interface. 

.. image:: ../manual-images/Network-Interfaces-Wan-Lan-example.png
  :width: 750
  :alt: Screenshot of the Interfaces page in the Wan/Lan example 

To demonstrate what is happening at the Linux level here is the configuration in the base operating system. The Linux *eth0*
interface has both the DHCP assigned IPv4 address and the DHCPv6 assigned IPv6 address bound.   

.. image:: ../manual-images/Linux-ifconfig-Wan-Lan-example.png
  :width: 700
  :alt: Screenshot showing how both the DHCP/DHCPv6 address is on a single Linux network Interface.

Understanding how Opuntia is interacting with the Linux networking stack is important if you plan to use custom iptables 
firewall rules, monitoring and interacting in the CLI. The Linux networking stack is unaware of Opuntia device names. 
So attempts to use the Opuntia device names from the CLI will fail.  

Protocol Configuraion
---------------------

Opuntia supports several different interface protocols. This protocol configuration setting configures the main operating
mode of the interface. Below are the most commonly used protocol types. 

* Static addresses
* DHCP client
* DHCPv6 client
* Unmanaged
* WireGuard VPN
* Link Aggregation (IEEE 802.3ad)
* PPPoE

We will cover each of these protocol types in detail. But there are other types that are supported but we are not documenting 
at this time due to lack of real world useage. If you believe that you are required to use one of these protocols and you are 
having difficulty plese contact ImageStream support at *support@imagestream.com*.  

To change the protocol setting of an interface first navigate the the Interface page in the Web GUI. 

Main Menu - *Network --> Interfaces*

There you will see a listing of all of the interfaces currently configured in the system. Below is an example of we are 
showing the interfaces page from the *Wan and Lan* example we used in talking about Opuntia interfaces. 

.. image:: ../manual-images/Network-Interfaces-Wan-Lan-example.png
  :width: 750
  :alt: Screenshot of the Interfaces page in the Wan/Lan example 

As you can see we have three defined interfaces; Wan, Wan_6 and Lan. This example has protocol DHCP configued on the *Wan* 
interface, DHCPv6 on the *Wan_6* interface and Static address protocol on the *Lan* interface. 

To change an interface to a different protocol click the "Edit" button for that interface. Then select the drop down box 
labled "Protocol". 

.. image:: ../manual-images/Network-Interfaces-Edit-Proto.png
  :width: 700
  :alt: Screenshot showing the Protocol dropdown box location

You can select your new protocol and you will have to *Save & Apply* the change in the Interfaces page before you will be 
able to configure settings for the newly selected protocol. 

Static addresses
################

The "Static Protocol" is allows for setting IPv4/IPv6 addresses and address ranges manually on an interface. This one of the 
most common configuration scenarios. This protocol is frequently used with *internal* RFC 1918 addresses and for upstream Internet 
connections. The "Static Protocol" also allows the configuration of DHCP/DHCPv6 servers. So this protocol type is almost 
universally used on at least one interface in any deployment. 

Web GUI
*******

The interface configuration is accessed by navigating to the Network interfaces page. 

Main Menu - *Network --> Interfaces*

Once you have navigated to the interfaces page, you can click on the "Edit" button to see the "General Settings" interface tab.   

.. image:: ../manual-images/Network-Interfaces-Static-Proto-IPv4.png
  :width: 700
  :alt: Screenshot of editing an interfaces with the static protocol 

In this screenshot you see the "General Settings" tab. This tab allows for the setting of static IPv4 and IPv6 addresses.
 

- General Settings (IPv4/IPv6 address, netmask and Custom DNS servers)
- Advanced Settings (Built-in IPv6, MAC address override and MTU override)
- Physical Settings (Bridging configuration and Interface Selection)
- Firewall Settings (Firewall zone assigned to the interface)
- DHCP Server (DHCP Settings and IPv6 specific configuration)

**IPv4**

IPv4 addresses are the most commonly configured static addresses.  When setting IPv4 address you are given the option of inputing 
the address and netmask separately or to use the CIDR list notation. ImageStream recommends using CIDR syntax as it's more human 
readable and less likely to result is the incorrect configuration of address ranges. 

To ensure that you are using CIDR List notation. Click the small check box at the end of the IPv4 address box. 

.. image:: ../manual-images/Network-Interfaces-Static-Proto-IPv4-CIDR.png
  :width: 700
  :alt: Screenshot showing the CIDR check box
  
.. note:: When operting is CIDR notation it's import to make sure that you click the "+" button after typing in the address or it will not be saved. 

This example shows an address that has **NOT** been saved correctly.

.. image:: ../manual-images/Network-Interfaces-Static-Proto-CIDR-not-saved.png
  :width: 700

And this example shows the address is correctly saved. You will see a new text box below all saved addresses. 

.. image:: ../manual-images/Network-Interfaces-Static-Proto-CIDR-saved.png
  :width: 700

Other important IPv4 settings include "IPv4 gateway". It's important to note that is  

**IPv6**

Given that IPv6 fundementatly support multiple addresses per interface CIDR List notation is the only option for manually setting 
IPv6 addresses. With IPv6 often you 


CLI
***

**IPv4** 

Here is a list configuration options. 

+---------------+----------------------+--------------------------------------------------+
| Name          | Type                 | Description of the command                       |
+===============+======================+==================================================+
| ipaddr        | ip address           | Ip address In CIDR or normal format              |
+---------------+----------------------+--------------------------------------------------+
| netmask       | netmask              | IPv4 Subnet mask                                 |
+---------------+----------------------+--------------------------------------------------+
| gateway       | ip address           | Default IPv4 gateway                             | 
+---------------+----------------------+--------------------------------------------------+
| broadcast     | ip address           | Broadcast IPv4 address                           | 
+---------------+----------------------+--------------------------------------------------+
| dns           | list of ip addresses | Dns Server List                                  | 
+---------------+----------------------+--------------------------------------------------+
| metric        | integer              | Route metric for this interface                  |
+---------------+----------------------+--------------------------------------------------+

**IPv6**

+---------------+----------------------+--------------------------------------------------+
| Name          | Type                 | Description of the command                       |
+===============+======================+==================================================+
| ip6addr       | ipv6 address         | IPv6 Address list in CIDR format                 |
+---------------+----------------------+--------------------------------------------------+

+---------------+----------------------+--------------------------------------------------+
+---------------+----------------------+--------------------------------------------------+
+---------------+----------------------+--------------------------------------------------+
+---------------+----------------------+--------------------------------------------------+
+---------------+----------------------+--------------------------------------------------+



DHCP Client
###########

Web GUI
*******

CLI
***

DHCPv6 Client
#############

Web GUI
*******

CLI
***

Unmanaged
#########


WireGuard VPN
#############

Web GUI
*******

CLI
***

Link Aggregation
################

Web GUI
*******

CLI
***

PPPoE
#####

Web GUI
*******

CLI
***


VLAN Configuraion
-----------------