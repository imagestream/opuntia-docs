===============================
Wireless Interface Configuraion
===============================

.. contents:: Table of Contents

Wireless Basics
---------------

Opuntia supports most common WiFi standards. Current hardware platforms support the Wi-Fi 4, Wi-Fi 5 and Wi-Fi 6 standards. 

WiFi operates in three main spectrums of radio frequency. The 2.4Ghz, 5Ghz and 6Ghz bands. These are further subdivided into 
channels. 

Wireless Interfaces in Opuntia
------------------------------

Opuntia has extensive support for Wireless networking. Depending on your hardware platform you may have 802.11n, 802.11ac 
or 802.11ax interfaces. These interfaces can be configured in several different operating modes. 

Wireless Status
---------------

Opuntia will display Wireless on the main status page and on Wireless configuration page.

On the main status page you will see a section for each radio interfaces in your system. Depending on your hardware platform, this
may be a single radio device or multiple.

In the example below, we show a system with two radio interfaces.

.. image:: ../manual-images/Status-Wifi-example.png
  :width: 700
  :alt: Screenshot showing Wireless Status 

In this example we see that radio0 is operating in the 2.4Ghz channel 1 band. We can see the SSID is set to "EVPN". In this example you
see that "Bitrate" is not set. This is because no clients are connected. 

In contrast; radio1 has two SSID's configured "EVPN5" and "notevpn". This radio is running in the 5Ghz band on channel 157. You do see 
a Bitrate in this case reporting 650Mbit/s. This is because we do have a client device connected to the EVPN5 SSID. This Bitrate matches
the client TX rate. 

In the Associated Stations section we can see the single connected client device. We can see useful information such as MAC-Address, Host 
Ip addresses, signal to noise, current RX/TX data rates, MCS Class and operating mode.

Wireless Configuraion
---------------------

To configure Wireless interfaces first navigate to the Wireless Overview page.

Main Menu - *Network -> Wireless*

Wireless Overview
#################

.. image:: ../manual-images/Network-Wireless-Overview.png
  :width: 700
  :alt: Wifi configuration Overview page

Wireless Scanning
#################

Opuntia supports scanning for local SSID's on each radio that is installed. This can be useful for finding existing WiFi networks to join. 
The Scan is limited to the radio doing the scan. In the example below we used radio0 which is operating in the 2.4Ghz spectrum. So we 
see Wireless networks in channel 1-11. 


.. image:: ../manual-images/Network-Wireless-Scan-example.png
  :width: 700
  :alt: Wifi scanning example 

