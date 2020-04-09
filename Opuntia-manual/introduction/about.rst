************
Introduction
************

.. contents:: Table of Contents

About Opuntia
-------------

Opuntia is high performance routing and edge computing platform based on OpenWrt. It includes
support for these major features. 

 - Support for IPv4/IPv6 connectivity
 - VPN client and server support for Ipsec, OpenVpn and Wireguard
 - Detailed usage statistics 
 - Free Range Routing (Dynamic routing support for BGP, Ospf, Eigrp and IS-IS)
 - Automation via Ansible 
 - Local Docker containers
 - k3s (Light weight Kubernetes)

This provides a robust platform for many modern IT applications. 

Overview
--------

This documentation is generic to all Opuntia hardware platforms. For specific information
on an Imagestream router please look the the correct quickstart guide. 
 
* :doc:`../../ev1000/quickstart`
* :doc:`../../ap3500/quickstart`
* :doc:`../../rr1000/quickstart`

Since this is generic documentation that describes all Opuntia based systems, 
you may find differences in Interface names, default configurations or other 
hardware/configuration changes depending on your hardware platform. All platforms 
share the list of software features but they may lack the hardware requirements to run all 
configurations or services.

Using this documentation
------------------------

Many of the tasks described in this documentation require navigating through the web GUI menu system. 
In this document, we will use the following syntax to describe navigating through the GUI menu. 

Main Menu - *System --> Administration* 

This would describe clicking on the "System" button on the top GUI menu and selecting the 
Administration link from the drop-down menu. 

When using the CLI there are often two methods to change configuration settings. The first method is to 
use the "uci" command to set configuration parameters. This tool is good for automation and scripting usage.
The second method is to edit the correct configuration file located in the "/etc/config/" directory. This is often
the most convenient method make configuration changes. But it's possible to input invalid configurations when directly
editing the settings. We will provide examples of each method when relevant.

Some configuration options from the command line and the Web GUI apply instantly when the configuration is saved. An 
example of this is the root password. Once input in the CLI or the web GUI the password is changed and no further 
saving needs to be done. This is in contrast to most configuration settings must be "applied" before they take place. 
Where possible this will be noted. 

When making changes from the CLI if the change is not instantly applied; it will be required to run the "reload_config" 
command. This is equivlent to clicking the "Save & Apply" in the Web GUI. Until this is done, these configue changes are
not applied to the system. 

When using the Web GUI it's possible to "Save & Apply" the configuration while by-passing normal configuration checks.
This can be usful in some situations. To do this, click the small downwards facing triangle on the "Save & Apply" button 
and select the option.  
