*****************
Accessing Opuntia
*****************

.. contents:: Table of Contents

Connectivity
------------

Opuntia can run on a wide varity of systems from wireless AP's, small routers, large systems and virtual machines. This wide varity
of equipment requires that specific access methods are going to depend on the platform that your Opuntia system is currently using.
Please reference the Quickstart guide for your specific device information. 

Access via Web GUI
------------------

The easiest way to access Opuntia is via the Web GUI. You can access the Web GUI using any Ipv4 or IPv6 ip addess that is assigned
to the Opuntia system. The *Wan* and any *Lan* interface will allow access to the Web GUI. This might be more a permisive access 
than some administrators perfer. Please see the following section for restricting access to the :ref:`Hardening-Web-GUI`. 

To access the Web GUI via IPv6 you need to use the syntax "https://[IPv6 addess]/". For example if you if your IPv6 management IP 
address was 2605:6:62:ba40::1 then you would use this url "https://[2605:6:62:ba40::1]/". 


Access via SSH
--------------

Opuntia supports ssh access to the Linux command line ( ash shell ). The   
:ref:`Hardening-SSH-Access`. 

We recommend adding ssh keys for normal operation. See this chapter for more details :ref:`Adding-SSH-Keys`. 


Access via Serial Console
-------------------------

For hardware platforms that include a Serial Console; you can connect to the Local Console using the Serial connection. This is 
typically running at a speed of 115.2kbps with 8n1.

:ref:`Hardening-Local-Console`

Access via Local Console
------------------------

