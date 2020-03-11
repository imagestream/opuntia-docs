**************************
AP3500 QuickStart Guide
**************************

 The AP3500 is configured by default to allow access via several different methods. 

 - Access via Ethernet using the Web Interface
 - Access via Ethernet using SSH

**Ethernet Connectivity with the AP3500**

The AP3500 has a single Ethernet interface. The default configuration of the AP3500 is to bridge the Ethernet interface 
with the wireless radio. Due to this default configuration the AP3500 defaults to acting as a DHCP client. It will receive 
an ip address from any existing DHCP servers that are present on the network connected to the Ethernet interface. 

If the AP3500 fails to obtain an DHCP lease within the first 60 seconds; the AP3500 will switch modes and it will assign 10.10.199.1/24 
to the Ethernet interface.


**Acceess via Ethernet using the Web Interface**

The factory configuration of the AP3500 has interface Eth2 specified as the management interface. 

This interface defaults to a network address range of 10.10.199.0/24 with the router configured as 10.10.199.1 
and supports DHCP client access. All of the Ethernet interfaces the Envoy support auto mdi/mdix so the 
interface can be connected directly to a client device or an Ethernet switch with a standard Ethernet 
cable.    

When connected to this management interface a client device which is configured to use DHCP will receive an address in the 10.10.199.0/24 
network. To access the login page use the following url: `https://10.10.199.1/ <https://10.10.199.1>`_  

The admin username is "**root**".  The default password is "**imagestream**". 

.. image:: ../images/Opuntia-default-login.png
  :width: 600
  :alt: Screenshot of the Opuntia login page
