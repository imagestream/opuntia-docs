======================
Firewall configuration
======================

.. contents:: Table of Contents

This chapter covers Firewall configuration and theory for Opuntia systems. 


.. _Zone-based-Firewall:

Zone based Firewall
-------------------

By default Opuntia allows access to the Web GUI on ANY IPv4/IPv6 interface address. The 


.. _Firewall-Limiting-External-access:

Limiting External access
------------------------

By default Opuntia is permisive with access to local services like the Web GUI and ssh login. While very useful for allowing 
administrator access to the equipment; this can be a security risk. This section of the manual will detail how to remove some of 
the default firewall rules that allow access to these services from remote. 

