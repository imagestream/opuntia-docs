=====================================
Wired Ethernet Interface Configuraion
=====================================

.. contents:: Table of Contents

Interfaces in Opuntia
---------------------

A familiar concept in computer networking is that of the "Interface". An "Interface" is a software construct that allows 
computer systems to interact with networks. An interface can represent a physical device like an Ethernet port. It can also 
represent virtual devices like a Loopback device. 

Opuntia extends this concept slightly. Opuntia is based on a Linux operating system. The base Linux system has many 
different network Interfaces. An Opuntia interface can be mapped to a Linux Interface. And The Opuntia Interface can also 
be a group of bridged Linux Interfaces. By bridging multiple interfaces together they all share the same network IP / IPv6
address space. And any host connected to any of these interfaces can communicate directly.   

For example a common configuration is to have a *Lan* and *Wan* network interface. The *Wan* interface is mapped to an 
Linux interface that is connected to an upstream Internet provider. The *Lan* interface is often a brige of all of the other
Ethernet interfaces on the router. This replicates a common CPE (Customer premises equipment) configuration where for 
providing an Internet connection. 

This relationship between an interface in Opuntia and the base Linux networking interface is a key concept that is also
needed to understand how to configure VLAN tags on Ethernet devices. This will be covered in detail in the VLAN section. 
But as a brief introduction it is the mapping of the Opuntia interface to the Linux networking interface that controls
if traffic leaving the system will be VLAN tagged. 

So Opuntia networking interfaces are either a 1:1, 1:many or even a many:1 mapping with respect to the Linux networking 
interfaces. The Opuntia interfaces have flexible naming making it easy to understand the purpose of an interface; for example
name of *Wan* vs a Linux device name of *enp97s0f0*. They are used to define membership of a firewall zone. 

In Opuntia each interface has an associated network protocol. Going back to the common example of a *Wan* interface. It is 
common to have the *Wan* interface configured with protocol "DHCP Client". This configuration results in the system making
an IPv4 DHCP request on the physical interface. If IPv6 DHCP is required it's common to have a *Wan_6* interface also 
configured. This Opuntia interface shares the same physical interface with the *Wan* interface. But it has a different
network protocol configuration in this case "DHCPv6 client". 

.. image:: ../manual-images/Network-Interfaces-Wan-Lan-example.png
  :width: 600
  :alt: Screenshot of the Interfaces page in the Wan/Lan example 


