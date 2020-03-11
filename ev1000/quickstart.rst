**************************
EV1000 QuickStart Guide
**************************

 The Envoy-1000 is configured by default to allow access via several different methods. 

 - Access via Ethernet using the Web Interface
 - Access via Ethernet using SSH
 - Access using the built-in Serial port  

**Acceess via Ethernet using the Web Interface**

The factory configuration of the Envoy-1000 has interface Eth2 specified as the management interface. This interface defaults to a network
address range of 10.10.199.0/24 with the router configured as 10.10.199.1 and supports DHCP client access. All of the Ethernet interfaces 
the Envoy support auto mdi/mdix so the interface can be connected directly to a client device or an Ethernet switch with a standard Ethernet 
cable.    

When connected to this management interface a client device which is configured to use DHCP will receive an address in the 10.10.199.0/24 
network. To access the login page use the following url: `https://10.10.199.1/ <https://10.10.199.1>`_  

The admin username is "**root**".  The default password is "**imagestream**". 

.. image:: ../images/Opuntia-default-login.png
  :width: 600
  :alt: Screenshot of the Opuntia login page




More code tests::

  ip addr show dev eth0
  ip neigh

After the code.


