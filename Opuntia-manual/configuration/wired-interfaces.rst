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

Other important IPv4 settings include "IPv4 gateway". It's important to note that this should only be set on a single interface
since this will set the global default IPv4 route for the system.   

**IPv6 with Prefix Delegation**

.. important:: With IPv6 deployments the majority of configurations will be using ISP provided network space. If your deployment uses provider delegated network Prefixes you **MUST** use the built IPv6 management options described below and DHCP server **MUST** be enabled on this interface. 

The Opuntia operating system includes an automatic system to manage IPv6 when you recieve a IPv6 Prefix delegation from an upstream
provider. This automatic system will ensure that the system is providing downstream clients with the correct IPv6 addresses and 
manages any changes in routing that may be required. IPv6 Prefix delegation is by far the most common configuration scenario if you 
are connecting to the IPv6 Internet. This is fundementatly a dynamic proccess that makes it impossible to set a static IPv6 address.

But the built-in IPv6 management system does allow for several tunable values that allows the system administrator to control the
deployment of IPv6 networks and addresses. In order of importance these options are; IPv6 assignment length, IPv6 suffix and IPv6
assignment hint. Each of these options will be discussed in detail in this section. It is important understand that in most 
common configurations the only settting that you are likely to configure is IPv6 assignment length. The other two values are likely
to remain unconfigured or in the default state. 

.. image:: ../manual-images/Network-Interfaces-Static-Proto-IPv6.png
  :width: 700

The "IPv6 assignment length" allows the administrator to chose the desired IPv4 prefix length for the interface. This setting is 
also used to signal to the Opuntia operating system to enable to built-in IPv6 management on this interface. Selecting any value
will disable the normal static IPv6 configuration options for setting a static IPv6 address, IPv6 gateway and IPv6 routed prefix.

IPv6 assignment length is typically set to 64 bits. A IPv6 Prefix length of 64 bits allows for the standard IPv6 address 
auto-configuration for most client devices (SLAAC and DHCPv6). To function correctly you must recieve a IPv6 Prefix delegation from 
an upstream provider.

For example if the Opuntia system recieved a IPv6 prefix delegation of 2605:540:1::/60 and we set the "IPv6 assignment length" to 
64 bits; Opuntia will configure one of the 16 /64 network ranges in the 2605:540:1::/60 delegation on this interface. If the 
upstream provider changes the IPv6 prefix delegation those changes will be automatically applied to all downstream devices. 

.. note:: Most clent operating sytems install IPv6 routes using the link local address of the router. So a human readable address on a interface is purely a management feature.

The "IPv6 suffix" sets the IPv6 Interface ID. This is the last 64bits of a IPv6 address. This allows the administrator to control 
the last part of an IPv6 address that is assigned. Given our example of receiving a IPv6 prefix of 2605:540:1::/64 if we were 
set the the "IPv6 suffix" to "::100:1"; the expected IPv6 address assigned to the interface would be 2605:540:1::100:1/64. This 
setting does have a default value of "::1" so in many cases you will not need to make adjustments to this setting if you want your
routers IPv6 to in ::1. 

Setting the IPv6 suffix setting is useful for network troubleshooting. It allows you to set the human readable IPv6 address that 
the router will use when being probed with standard troubleshooting tools like traceroute and ping.  

.. important:: If a "IPv6 assignment hint" is outside of the IPv6 prefix ranges that are available this setting will have no effect.

If we wanted to control which /64 IPv6 prefix will be selected we can use the second setting "IPv6 assignment hint". This is an 
optional value, the default is not set. If this option is in the default state, the system will try to effecently allocate IPv6 
networks. If control of the assigned network is required; the value is a hex number that matches sub-Prefix ID. So in 
this example if we want to assign 2605:540:1:2::/64 we could set the hint value to "2". Or if it was required to assign 
2605:540:1:f::/64 we would set the value to "f". 

Given the dynamic nature of IPv6 prefix delegation it is often not required to control the specific network. Also it's important to
remember that this is a "hint" that requires the expected network address to be included in your IPv6 prefix delegation. If your
provider adjustes the assigned prefix delegation it is quite possible that your "hint" will no longer be able to map to a valid 
network range. That would result this setting having no effect. For this reason we suggest not using this feature if you are 
receiving a Prefix Delegation. But it is useful in a few deployment scenarios so it's usage is detailed here. 

**DHCP/DHCPv6 settings - Static protocol**

.. note:: It is a required that you use DHCP server settings on the interface for downstream devices to recieve IPv6 prefix delegation. 

Client devices are almost never statically assigned IPv6 addresses and if you are using the recommended Opuntia built-in IPv6 
management to delegate IPv6 Prefixes you must configure DHCP/DHCPv6 server on this interface. This section will contain only a 
brief list of commands and settings needed to ensure that client devices will function. For a full description of the DHCP / DHCPv6
server Interface settings and Global DHCP settings please look at the DHCP Server chapter linked below.

:doc:`dhcp-server`

.. image:: ../manual-images/Network-Interfaces-Static-DHCP-unconfig.png
  :width: 700
  :alt: Screenshot of the DHCP tab before being configured

To begin, Edit the interface and click to the "DHCP Server" tab. You will see a large button labled "Setup DHCP Server".

.. image:: ../manual-images/Network-Interfaces-Static-DHCP-Gen.png
  :width: 700
  :alt: Screenshot of the DHCP General Setup tab

This tab shows the basic DHCP server settings for the interface. 

- Ignore interface
- Start
- Limit
- Lease time

The "Ignore interface" checkbox will disable the IPv4 DHCP server on this interface. If selected this will automatically hide the 
"advanced settings" tab. This can be a useful configuration option if you are manually assiging IPv4 addresses but you want to use
the built-in IPv6 subsystem. 

The "Start" configuration setting specifies where in the IPv4 network range to begin allocating DHCP client ip addresses. For 
example if you have a static network of 192.168.85.0/24 allocated to the interface and you have the "Start" setting set to 100; the
lowest ip address that can be allocated is 192.168.85.100. This is a required setting if DHCP server is enabled. 

The "Limit" setting works with the start value to limit how many addresses can be allocated and thereby defining the DHCP addresses
that can be allocated. In our example of starting our DHCP pool at 192.168.85.100 if we use the default value of 150 ro the "Limit" 
setting that results in the system allocating Ip addresses from 192.168.85.100 to 192.168.85.250. Or otherwise limiting the 
allocation to 150 addresses above the "Start" setting. This is a required setting if DHCP server is enabled. 

The "Lease time" setting defines the length of time that the DHCP Lease is valid. This is a required setting if DHCP server is 
enabled.  

To configure DHCPv6 settings click the "IPv6 Settings" tab. 

.. image:: ../manual-images/Network-Interfaces-Static-DHCPv6.png
  :width: 700
  :alt: Screenshot of the DHCPv6 settings

On this tab we see the DHCPv6 settings. The most commonly used settings are as follows.  

- Router Advertisement-Service
- DHCPv6-Service
- DHCPv6-Mode
- Announced DNS servers

The "Router Advertisement-Service" enables the Opuntia system to send IPv6 router Advertisement packets on this interface. This 
allows client devices to learn that the Opuntia system is acting as a router for this network. It also serves as the primary 
enabler for the usage of SLAAC (Stateless address autoconfiguration) to automatically configure IPv6 networks. The recommended 
value is "Server mode". 

The "DHCPv6-Service" is the setting that actually starts DHCPv6 on the Interface. The recommended value is "Server mode".

The "DHCPv6-Mode" controls the operating mode of the DHCPv6 Server. This settting will be explained in more detail in the DHCP 
Server chapter. The recommended value is "Stateless + Stateful". 

The last most commonly configued IPv6 DHCPv6 is "Announced DNS servers". This value is very similar to setting the DNS server 
option for IPv4. This allows the DHCPv6 clients to learn a list of DNS servers. This is an optional setting since you may learn 
DNS servers from IPv4 DHCP or other methods. 

One interesting thing about the "Announced DNS servers" setting is that you can announce IPv4 or IPv6 DNS servers addresses using 
this configuration value. Depending on the enviorment, it may be valid to only have IPv4 DNS servers specified in the IPv6 DHCPv6 
service. 

.. image:: ../manual-images/Network-Interfaces-Static-DHCPv6-dns.png
  :width: 700
  :alt: Screenshot showing IPv4 and IPv6 DNS servers being Announced using DHCPv6. 
 

**Static IPv6**

There are several deployment scenarios where you will not recieve an IPv6 prefix delegation. Typically this is when you are learning
IPv6 routes over a dynamic routing protocol such as BGP or Ospfv3. In these cases you are required to set a IPv6 manually on 
interfaces. To get started, you first must be sure that the "IPv6 assignment length" setting is set to disabled.

.. image:: ../manual-images/Network-Interfaces-Static-Proto-IPv6-Prefix-disabled.png
  :width: 700
  :alt: Screenshot of the Interface General settings with a disabled IPv6 Prefix length.

By disabling the "IPv6 assignment length" you will now see these configuration options. 

- IPv6 address
- IPv6 gateway
- IPv6 routed Prefix
- IPv6 suffix

Given that IPv6 fundementatly supports multiple addresses per interface; CIDR List notation is the only option for manually setting 
IPv6 addresses. Be sure to click the small check box at the end of the IPv6 address box. Below is an example of adding multiple 
IPv6 addresses to an interface. 

.. image:: ../manual-images/Network-Interfaces-Static-Proto-IPv6-example.png
  :width: 700
  :alt: Screenshot of adding two IPv6 Address manually

The "IPv6 routed Prefix" is used with the built-in IPv6 management system. This allows the system administrator to specify a 
static IPv6 Prefix that is distrubted to clients devices using DHCPv6. DHCPv6 **MUST** enabled for this setting to be effective. 
Since this prefix range is specified manually by the administrator; the administrator must ensure that this IPv6 Prefix range is 
routed to the Opuntia system. This can be done using dynamic routing protocols like BGP, Ospfv3 or static routes. 

The "IPv6 suffix" sets the IPv6 Interface ID. This is the last 64bits of a IPv6 address. This allows the administrator to control 
the last part of an IPv6 address that is assigned. This setting only takes effect if you have also specified a "IPv6 routed Prefix". 
This setting has a default value of "::1". 

CLI
***

**IPv4** 

.. image:: ../manual-images/Network-Interfaces-Static-Proto-IPv4-cli.png
  :width: 700
  :alt: Screenshot showing the CIDR check box

And here is an example setting two static IPv4 addresses on interface named "Home_Lan".  

.. code-block:: python
  :emphasize-lines: 4-5
     
  config interface 'Home_Lan'
        option ifname 'eth1'
        option proto 'static'
        list ipaddr '192.168.85.1/24'
        list ipaddr '192.168.86.1/24'
        list dns '192.168.85.10'



Here is a list of common configuration options and value descriptions. 

+---------------+----------------------+----------+--------------------------------------------------+
| Name          | Type                 | Required | Description of the command                       |
+===============+======================+==========+==================================================+
| ifname        | Interface Name       | Yes      | Physical Interface Name                          |
+---------------+----------------------+----------+--------------------------------------------------+
| proto         | Protocol Type        | Yes      | Protocol                                         | 
+---------------+----------------------+----------+--------------------------------------------------+
| ipaddr        | ip address           | Yes      | Ip address CIDR list                             |
+---------------+----------------------+----------+--------------------------------------------------+
| netmask       | netmask              | No       | IPv4 Subnet mask                                 |
+---------------+----------------------+----------+--------------------------------------------------+
| gateway       | ip address           | No       | Default IPv4 gateway                             | 
+---------------+----------------------+----------+--------------------------------------------------+
| broadcast     | ip address           | No       | Broadcast IPv4 address                           |
+---------------+----------------------+----------+--------------------------------------------------+ 
| dns           | list of ip addresses | No       | Dns Server List                                  | 
+---------------+----------------------+----------+--------------------------------------------------+
| metric        | integer              | No       | Route metric for this interface                  |
+---------------+----------------------+----------+--------------------------------------------------+

**IPv6**

+---------------+----------------------+----------+-----------------------------------------------------+
| Name          | Type                 | Required | Description of the command                          |
+===============+======================+==========+=====================================================+
| ifname        | Interface Name       | Yes      | Physical Interface Name                             |
+---------------+----------------------+----------+-----------------------------------------------------+
| proto         | Protocol Type        | Yes      | Protocol                                            | 
+---------------+----------------------+----------+-----------------------------------------------------+
| ip6addr       | ipv6 address         | Yes / No*| IPv6 Address (not required if an IPv4 ipaddr is set)|
+---------------+----------------------+----------+-----------------------------------------------------+
| ip6gw         | ipv6 address         | No       | IPv6 Default Gateway                                |
+---------------+----------------------+----------+-----------------------------------------------------+
| ip6assign     | prefix length        | No       | Delegate a prefix of this length                    |
+---------------+----------------------+----------+-----------------------------------------------------+
| ip6hint       | prefix hint          | No       | Prefix hint in Hex format                           |
+---------------+----------------------+----------+-----------------------------------------------------+
| ip6class      | ipv6 prefix          | No       |                                                     |
+---------------+----------------------+----------+-----------------------------------------------------+
| ip6prefix     | ipv6 prefix          | No       | IPv6 prefix for distribution to clients devices     |
+---------------+----------------------+----------+-----------------------------------------------------+


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