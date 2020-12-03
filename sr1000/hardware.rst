*******************************
Samurai Router Hardware Details
*******************************

.. contents:: Table of Contents

Samurai Router
--------------

The Samurai Router is the top of the line ImageStream Router. 

Key features include
 
 - Powerful 8 Core CPU
 - Two 10Gbase-T 10Gbit Interfaces
 - Two Sfp+ 10Gbit Interfaces
 - Redundant AC Power Supplies
 - four front pannel expansion slots
 

Front ports
-----------

.. image:: images/SR1000-Front-Ports-LEDs.png
  :width: 700
  :alt: image showing the front ports on the Samurai Router


.. table:: Front Ports and LEDs

   +-----------+-----------------+------------------+----------------+----------------+
   | Diagram # | Case Label      | Opuntia Dev Name | Linux Dev Name | Description    |
   +===========+=================+==================+================+================+
   | 1         | Console         | n/a              | /dev/ttyS0     | Serial Console |
   +-----------+-----------------+------------------+----------------+----------------+
   | 2         | MGMT            | Managment        | enp2s0         | 1Gbit Ethernet |
   +-----------+-----------------+------------------+----------------+----------------+
   | 3         | USB             | n/a              | n/a            | USB 3.0 Type A |
   +-----------+-----------------+------------------+----------------+----------------+
   | 4         | Console USB     | n/a              | n/a            | USB Console    |
   +-----------+-----------------+------------------+----------------+----------------+
   | 5 (bottom)| IPMI/10GbE 1    | Managment-10g    | enp183s0f2     | 10Gbit Ethernet|
   +-----------+-----------------+------------------+----------------+----------------+
   | 5 (top)   | 10GbE 2         | Eth2             | enp183s0f3     | 10Gbit Ethernet|
   +-----------+-----------------+------------------+----------------+----------------+
   | 6 (bottom)| SFP+ 1          | Sfp1             | enp183s0f1     | SFP+ Interface |
   +-----------+-----------------+------------------+----------------+----------------+
   | 6 (top)   | SFP+ 2          | Sfp2             | enp183s0f0     | SFP+ Interface |
   +-----------+-----------------+------------------+----------------+----------------+
   | 7         | Status LED      |                  |                | Status LED     |
   +-----------+-----------------+------------------+----------------+----------------+
   | 8         | UID LED         |                  |                | UID LED        |
   +-----------+-----------------+------------------+----------------+----------------+
   | 9         | HDD LED         |                  |                | HDD LED        |
   +-----------+-----------------+------------------+----------------+----------------+
   | 10        | Reset LED       |                  |                | Reset LED      |
   +-----------+-----------------+------------------+----------------+----------------+
   | 11        | Power LED/Button|                  |                | Status LED     |
   +-----------+-----------------+------------------+----------------+----------------+

.. note:: The Managment and Managment-10g interfaces may be used as normal interfaces in Opuntia. But the Managment-10 Interface will always expose the system BMC IPMI. So care should be taken to provide the needed security if using this interface.


