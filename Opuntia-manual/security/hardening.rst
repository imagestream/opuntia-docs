==================
Security Hardening
==================

.. contents:: Table of Contents

This chapter covers Security Hardening prodcures for Opuntia systems. By default Opuntia is configured to allow easy access to the 
system for administration. But these settings were chosen to favor access over security concerns. Opuntia does not come with an 
insecure configuration; but it allows access to login prompts in several situations that administrators should consider limiting
access in higher security configurations. 

Web GUI
-------

By default Opuntia allows access to the Web GUI on ANY IPv4/IPv6 interface address. We recommend excluding access to the Web GUI to
the default *Wan* Firewall zone. Please see the :ref:`Zone-based-Firewall` chapter for more information.

Serial / Local Console
----------------------

.. note:: This requires Opuntia version 4.8.16 r49400 or newer.

By default Opuntia does not require a password when accessing system from a Serial or Local console. Since this requires physical 
access to hardware platforms this is generally considered safe. But Virtual Machines running Opuntia often do have remote methods
to access the Local console. So this is an important security concern. 

To require a password for Serial / Local Console logins use the following prodcure. 

To make these adjustments use the following commands from the CLI ::

    uci set system.@system[0].ttylogin="1"
    uci commit system
    service system restart
