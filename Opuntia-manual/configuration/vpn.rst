================
VPN Configuraion
================

.. contents:: Table of Contents

Opuntia VPN
-----------

Opuntia supports a wide variety of VPN technologies. The main options for Opuntia are the following Vpn types. 
  
* Wireguard
* OpenVpn
* IPsec 
* IPsec/L2TP

We will detail each option in a section of this chapter of the Opuntia manual. 

Wireguard
---------

OpenVpn
-------

IPsec
-----

IPsec/L2TP
----------

IPsec/L2TP is a widely deployed VPN technlogy since client support is built into most operating systems. It features the 
strong security of IPsec with the addition of the the L2TP tunneling protocol. This does add complexity to the already 
feature rich IPsec vpn. This allows for a username and password to be asigned each user. This multi-factor VPN 
configuraion is required in some remote security enviorments. 

Currently as of Opuntia version 4.8.17 this configuration has not recieved a Web GUI configuration menu. We will outline
CLI method to configure this VPN type. To successfully configure IPsec/L2TP you will need the following items. 

* Public IP Address
* Pre-Shared Key 
* Username for each user
* Password for each user

Since the default options for IPsec/L2TP is to automatically negotiate most of the options for this type of VPN the 
total configuration is not that complex. There are four main files that need to be edited. 

 The first file that must be edited is /etc/ipsec.secrets. 

.. code-block:: python
  :caption: /etc/ipsec.secrets
  :emphasize-lines: 2
     
  # /etc/ipsec.secrets - strongSwan IPsec secrets file
  203.0.113.1   %any : PSK "3dTamd01m"

This file configures your Public IP address and the Pre-Shared Key. In this example the 203.0.113.1 address is your 
public IP address that the clients will be connecting to. The "%any" allows any Ip Address to connect to this VPN. 
This can be used to limit connecting clients but is generally set to "%any" in most vpn configurations. 

The Pre-Shared Key in this example is a short text string *3dTamd01m* this string can be any valid ASCII string. 

 The second file is the main IPsec configuraion file /etc/ipsec.conf 

 .. code-block:: python
   :caption: /etc/ipsec.conf
   :emphasize-lines: 9-18

   # ipsec.conf - strongSwan IPsec configuration file
   # basic configuration

   config setup
        # strictcrlpolicy=yes
        # uniqueids = no

    # Add connections here.
    conn vpnserver
        type=transport
        authby=secret
        rekey=no
        keyingtries=1
        left=203.0.113.1
        leftprotoport=udp/l2tp
        right=%any
        rightprotoport=udp/%any
        auto=add

Here you see the IPsec Pre-Shared Key configuraion named *vpnserver*. This defines the IPsec parmeters for the connection. 
Notable options specified are *authby=secret*, *left=203.0.113.1*, *right=%any* and both of the right and left protocol 
ports. 

The *authby* setting tells the IPsec subsystem to use the Pre-Shared Key that we set in the /etc/ipsec.secrets file. The 
*left* setting defines the Public IP Address that the system is using and this *right* allows any client to connect. The 
two protocolport options specify that only UDP port 1701 will be encrypted using IPsec. 

This ends the IPsec configuraion that needs to be configured. 

 The next two files deal with the L2TP configuraion. First up is the /etc/xl2tpd/xl2tpd.conf configuraion file.

 .. code-block:: python
   :caption: /etc/xl2tpd/xl2tpd.conf 
   :emphasize-lines: 4,10-11,17

   [global]
   port = 1701
   auth file = /etc/xl2tpd/xl2tp-secrets
   listen-addr = 203.0.113.1
   access control = no
   debug tunnel = no

   [lns default]
   exclusive = yes
   local ip = 192.168.88.1
   ip range = 192.168.88.10-192.168.88.200
   hidden bit = no
   length bit = yes
   require chap = yes
   refuse pap = yes
   name = vpn
   ppp debug = yes
   pppoptfile = /etc/ppp/options.xl2tpd

This is the L2TP server configuraion file. Here you see the Public IP Address referenced in the  global *listen-addr* 
setting. The default lns (L2TP server) specifies the local IP Address that will be assigned to the router and to the
client devices. 

In this example; we have chosen to set the router's IP Address to 192.168.88.1 and we allow clients to use a range
of 192.168.88.10 through 192.168.88.200. If you are accessing other internal networks; you may need to add a 
route to this network from other devices. 

One other setting of note is the *ppp debug = yes* option. This will cause verbose system logs to be generated 
when clients connect to the vpn service.

 The final configuration file defines the users and passwords that the clients need to provide to access the VPN.  

 .. code-block:: python
   :caption: /etc/ppp/chap-secrets 
   :emphasize-lines: 3-4

   # Secrets for authentication using CHAP
   # client       server         secret               IP addresses
   alice         vpn            47roses            *
   bob           vpn            D3adB33f           *

This file is the standard chap-secrets format. Here we have two users setup; alice and bob. We see the server setting is 
*vnp* which is the name of the default lns set in our example xl2tpd.conf file from above. We also see the two 
passwords that these users have chosen. This "*" setting allows the client to get any IP address from the pool 
that we have defined. 

Firewall configuraion
*********************

By default Opuntia needs to be configured to allow incoming L2TP connections in the *wan* firewall zone. If see the 
firewall chapter for more general information. 

The rule we need to add is an accept rule for the *Wan* input zone that allows TCP and UDP port 1701 traffic.

To navigate to the Firewall configuraion page. 

Main Menu - *Network --> Firewall --> Traffic Rules*

.. image:: ../manual-images/Firewall-Rules-L2TP.png
  :width: 600
  :alt: L2TP Firewall ruleset

Once that rule is installed you will see this from the firewall ruleset summary. 

.. image:: ../manual-images/Firewall-Rules-L2TP-Applied.png
  :width: 600
  :alt: L2TP Firewall ruleset











 





