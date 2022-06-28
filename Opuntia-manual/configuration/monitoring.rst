==========
Monitoring
==========

.. contents:: Table of Contents


Monitoring Opuntia systems
--------------------------



Remote Packet Capture
---------------------

Using ssh it's possible to do a remote packet capture and use a local tool like WireShark to view the packet 
capture in real time. 

.. code-block:: bash

  ssh 192.168.77.1 "tcpdump -s0 -i eth1.79 -w - 'port 53'" | wireshark -k -i -

What we have here is a quick example. We ssh to the remote router. In this case an Envoy at 192.168.77.1. 

Then we run tcpdump; in this example we are capturing only DNS port 53 packets on interface eth1.79. We 
specified the "-w -" option to have tcpdump the packets out as a file but we then direct the output into a 
locally running wireshark process. This will cause the wireshark display in realtime the packets that 
match our tcpdump filter from the remote router. 

Windows
=======

This example above requires a Linux enviorment to work correctly. If the user has a Windows system, it should
still be possible to do similar commands using powershell. But it's also possible to configure WSL or WSL2 
and run wireshark in the linux enviorment directly as outlined above. 
