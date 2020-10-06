*****************
Accessing Opuntia
*****************

.. contents:: Table of Contents

.. _Access-ToC: 

Connectivity
------------

Opuntia can run on a wide varity of systems from wireless AP's, small routers, large systems and virtual machines. This wide varity
of equipment requires that specific access methods are going to depend on the platform that your Opuntia system is currently using.
Please reference the Quickstart guide for your specific device information. 

.. _Access-Web-GUI:

Access via Web GUI
------------------

The easiest way to access Opuntia is via the Web GUI. You can access the Web GUI using any IPv4 or IPv6 ip addess that is assigned
to the Opuntia system. The *Wan* and any *Lan* interface will allow access to the Web GUI. This might be more a permisive access 
than some administrators perfer. Please see the following section for restricting access to the :ref:`Hardening-Web-GUI`. 

To access the Web GUI via IPv6 you need to use the syntax "https://[IPv6 addess]/". For example if you if your IPv6 management IP 
address was 2605:6:62:ba40::1 then you would use this url "https://[2605:6:62:ba40::1]/". 

.. important:: Opuntia by default ships with a Self Signed SSL certificate. This will likely cause a security error when accessing the Web GUI. Given that Opuntia devices can not be assumeed to always have Internet access we are currently unable to include a updated valid certificate for every device. ImageStream is currently working to resolve this issue. 

.. _Access-SSH:

Access via SSH
--------------

Opuntia supports ssh access to the Linux command line ( ash shell ). Opuntia by default allows SSH access on all localy 
configured IPv4/IPv6 addresses. This allows easy access for system administrators but my not be the perfered configuration 
from a security standpoint. Please see the :ref:`Hardening-SSH-Access` chapter if this default is not acceptable.   

In all cases recommend adding ssh keys for normal operation. See this chapter for more details :ref:`Adding-SSH-Keys`. 

.. _Access-Serial:

Access via Serial Console
-------------------------

For hardware platforms that include a Serial Console; you can connect to the Local Console using the Serial connection. This is 
typically running at a speed of 115.2kbps with 8n1. Depending on the hardware platform you may need a null Serial connection. 
Please check with your Quickstart guide to see if this is the case. 

.. important:: A common issue with Serial connections is cables the lack connected pins this can cause the connection to fail in unexpected ways (for example seeing output but not being able to send keystrokes). Take care you a using the correct cable for your hardware platform. 

The specific connector and cable combination will depend on the hardware platform. If your hardware platform supports an RJ-45 
Serial connection, the pin-out will be as follows. 

.. table:: RJ-45 to DE-9 Pinout Table

  +-----------------+----------+-------------------+------------+-------------------+
  | RJ-45 Cable Pin | Color    | RJ-45 Definition  | DE-9 Pin   | DE-9 Definition   |
  +-----------------+----------+-------------------+------------+-------------------+
  | 1               | Brown    | CTS               | 8          | RTS               |
  +-----------------+----------+-------------------+------------+-------------------+
  | 2               | Purple   | DSR               | 6          | DSR               |
  +-----------------+----------+-------------------+------------+-------------------+
  | 3               | Yellow   | RXD               | 2          | TXD               |
  +-----------------+----------+-------------------+------------+-------------------+
  | 4               | Red      | GND               | 5          | GND               |
  +-----------------+----------+-------------------+------------+-------------------+
  | 5               | Black    | GND               | 5          | GND               |
  +-----------------+----------+-------------------+------------+-------------------+
  | 6               | Orange   | TXD               | 2          | RXD               |
  +-----------------+----------+-------------------+------------+-------------------+
  | 7               | Blue     | DTR               | 4          | DSR               |
  +-----------------+----------+-------------------+------------+-------------------+
  | 8               | Green    | RTS               | 7          | CTS               |
  +-----------------+----------+-------------------+------------+-------------------+
  |                 | N/A      | No Connect        | 1, 9       | No Connect        |
  +-----------------+----------+-------------------+------------+-------------------+

Once you have a Serial connection established, you can press any key to activate the Serial console. 

By default the Serial console has no security. Security is provided by the requirement that accessing the Serial console requires 
phyical localiticy to the Opuntia system. This my not always be true; for example if you are access the Serial console using a 
remote system. In this case it is highly recommend that you require password login on the Serial console please see the 
:ref:`Hardening-Local-Console` chapter for more information. 

.. _Access-Local:

Access via Local Console
------------------------

On hardware platforms that support USB and Video connections. Accessing the CLI is very simple. Just connect a keyboard and
monitor to the system and then press any key on the keyboard. This will spawn a new ash CLI shell. 

By default the Local console has no security. Security is provided by the requirement that accessing the Local console requires 
phyical localiticy to the Opuntia system. This my not always be true; for example if you are access the Local console using a 
remote system. In this case it is highly recommend that you require password login on the Local console please see the 
:ref:`Hardening-Local-Console` chapter for more information. 
