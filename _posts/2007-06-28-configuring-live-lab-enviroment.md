---
layout: post
title:  "Configuring Live Lab Environment"
date:   2007-06-28 08:56:08 -0600
categories: blog
tags: June 2007 KU SOECS Lab
---
Live Lab

   1. Imaged Servers
   2. Configured Network Card Drivers
   3. Created soecs.lab domain
   4. Joined RIS to domain
   5. Installed RIS, and configured first Image (Windows XP)
   6. Connected Precision 390 to Lab

The Dell Precision 390 has the same Broadcom 57xx NIC as the OptiPlex 745 that I used for my Live Servers, so I followed the same instructions and added the appropriate files to the appropriate places, and modified all the correct files to make the NIC work. All that information is in the Virtual Lab Blog entry.

I have had little luck in the past getting the Precision 390 to do an unattended install due to the SATA drivers. After some digging online I came across this [article](http://www.dellcommunity.com/supportforums/board/message?board.id=pw_harddrive&message.id=1749&query.id=74107#M1749) , apparently there is a BIOS option for the SATA (http://www.intel.com/technology/serialata/) controller to switch it from [AHCI](http://www.intel.com/technology/serialata/ahci.htm) mode to ATA mode. When the SATA controller is in AHCI mode a driver is required, and I have yet to figure out how Dell does that, as none of the available Dell drivers for this computer seem to accommodate an unattended installation. When the controller is in ATA mode no driver is required as ATA is fully supported in XP. In order for this to work properly all Dell Precision 390’s must be switched to ATA mode. I fully expect that when we move to Windows Vista we can switch back to AHCI mode.

The next test is to create the base SOECS image.
