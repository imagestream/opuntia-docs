=====
Ipsec
=====

.. contents:: Table of Contents



Advanced
--------

Ipsec needed documentation item:

 /etc/strongswan.d/charron.conf

 This file has many extra configuration options that can not normally be configured. Most of these options will not be needed. But a few are 
 critical in some specific situations. For example ignore_routing_tables command. This command allows you to prevent Ipsec from looking at 
 a specific routing table when doing split vpn routing. This example below is from when there was a Openvpn VPN tunnel already installing a 
 default route into table 100. This default route was *NOT* installed into the main routing table. IPsec would see this route and then build 
 the IPsec tunnel using this default route. The result was the IPsec was tunneled over the Openvpn.  

 ignore_routing_tables = 100