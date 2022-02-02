---
layout: post
title:  "After Action Notes"
date:   2007-07-18 09:03:00 -0600
categories: KU SOECS Lab
tags: July 2007
---
Problems:

* All computers need to be configured to PXE boot
* Dell Precision 390 desktops need to have SATA configured for ATA operation in order for RIS to work
* Updated RIS images need to be modified to have the Broadcom 57xx driver
* RIS has a limitation on the master image machine, if the drive is larger than the drive on the destination computer the image won’t deploy even if there is enough room on the destination computer.

Thoughts:

* Planning for the future, Vista can only be deployed via WDS
* Convert RIS to WDS on Server 2003
* Compare Windows Server 2003 WDS with the Windows Deployment Role in Windows Server 2008
* Extract MAC addresses from all SOECS computers from ANSR
* Create a script to manage DHCP
  * Bring DHCP online for deployment
  * Bring DHCP offline after deployment
