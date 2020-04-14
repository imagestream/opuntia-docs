=======================
Configuration Managment
=======================

.. contents:: Table of Contents

This chapter will detail how to back-up, restore and reset to defaults of your system configuration. 

Configuration Backup
--------------------

**Web GUI**

The easiest wasy to get a backup of an Opuntia system is to use the Backup option on the "Flash operations" page.

Main Menu - *System --> Backup/Flash Firmware*

To get a backup; simply click the "Generate archive" button. This will cause the system to generate a backup file using 
the following format. "backup-<Current hostname>-<Current Date>.tar.gz". This file will be downloaded by your Web Browser.

.. image:: ../manual-images/System-Backup.png
  :width: 600
  :alt: The Backup/Restore and flash page

**CLI**

The process to backup using the CIL is more involved, you first must manually generate the backup file. And then transfer 
the file off of the system. ::

  sysupgrade -b /tmp/backup-${HOSTNAME}-$(date +%F).tar.gz

This will generate a backup file that matches the same format you get when using the Web GUI method. But it is possible to
use whatever filename you wish. 

.. note:: If you do generate a backup file make sure you use the /tmp/ directory as the destination 

.. image:: ../manual-images/Backup-CLI.png
  :width: 500
  :alt: Screenshot of a manual backup on the CLI.

Once you have created a backup image you must transfer it off of the system. The simplest way to do this is to use the 
"scp" command. This allows you to transfer the image off of the system and to a remote host. 

Restoring Configurations
------------------------

**Web GUI**

To restore the system from a downloaded configuration file, first navigate to the "Flash operations" page. 

Main Menu - *System --> Backup/Flash Firmware*

.. image:: ../manual-images/System-Backup.png
  :width: 600
  :alt: The Backup/Restore and flash page

There are two options for restoring the system. You can restore from a saved backup file or you can restore the system
to the default configuration. 


.. image:: ../manual-images/System-Backup-uploading.png
  :width: 600
  :alt: Screenshot of the backup uploading

.. image:: ../manual-images/System-Backup-restore.png
  :width: 600
  :alt: Screenshot of the backup restore confermation page

**CLI**

Upgrading the OS 
----------------

**Web GUI**

