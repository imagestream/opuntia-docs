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
The color coded of the devices tell you which firewall zone applies to the interface. 

.. image:: ../manual-images/Network-Interfaces-Wan-Lan-example.png
  :width: 750
  :alt: Screenshot of the Interfaces page in the Wan/Lan example 

To demonstrate what is happening at the Linux level here is the configuration in the base operating system. The Linux *eth0*
interface has both the DHCP assigned IPv4 address and the DHCPv6 assigned IPv6 address bound.   

.. image:: ../manual-images/Linux-ifconfig-Wan-Lan-example.png
  :width: 700
  :alt: Screenshot showing how both the DHCP/DHCPv6 address is on a single Linux network Interface.

Understanding how Opuntia is interacting with the Linux networking stack is important if you plan to use custom iptables 
firewall rules, monitoring and in the CLI. The Linux networking stack is unaware of Opuntia device names. So attempts to 
use the Opuntia device names from the CLI will fail.  

Protocol Types
--------------



Static Protocol
---------------

The "Static Protocol" is allows for setting IPv4 and IPv6 addresses manually on an interface. This one of the most common 
configuration scenarios for interal 

.. image:: ../manual-images/Network-Interfaces-Static-Proto-IPv4.png
  :width: 700
  :alt: Screenshot of editing an interfaces with the static protocol 
