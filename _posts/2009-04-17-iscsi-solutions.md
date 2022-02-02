---
layout: post
title:  "iSCSI Solutions"
date:   2009-04-17 14:03:00 -0600
categories: blog
tags: April 2009 KU SOECS
---
iSCSI provides a cost-effective, unified method for accessing storage from various vendors. My example is an old free-standing Compaq Drive array and a rack mounted Dell Drive array. Both of these attach to the head server via regular SCSI. Keep in mind that the local connections to the data doesn’t matter, they could be external SAS enclosures or SATA drives, the “SCSI” part of iSCSI is an over the network thing and not a local requirement thing.

Ideally Microsoft would provide this software as a purchased product or an add-on but sadly it is only available via OEM channels, which usually means it’s bundled in an iSCSI solution. Don’t get me wrong there are several very nice iSCSI solutions from all the big vendors. But if cost is important to you then the cheaper you can get it, the better.

There are two methods that I’ll cover the first is a Windows solution using freely available iSCSI Target software from MySAN, and the other is a Linux solution using  completely free software available from anywhere. For my purposes I’ll be covering Ubuntu 8.04 LTS.

I will provide links to all the software that I mention in the links section below.

NIMBUS

---
From the Nimbus website.

> Nimbus Data Systems, Inc. develops Unified Storage systems and software that dramatically simplify storage management, lower operating costs, and improve IT availability. Nimbus Unified Storage is the premier storage infrastructure for server and desktop virtualization, rich content, cloud computing, storage consolidation, and high-performance computing. To date, over 15,000 companies in 28 countries have implemented Nimbus technology.

In 2006 Nimbus released free iSCSI target software for use in Microsoft Windows.

> San Francisco, CA, August 14, 2006 – Nimbus Data Systems today announced MySAN™, the first and only free iSCSI target software for Microsoft Windows. With MySAN, anyone can create an IP SAN in seconds using their existing server and storage hardware. MySAN works by turning any Windows partition (such as a hard drive, internal RAID array, external storage system, or even Fiber Channel storage) into an iSCSI target. This storage can then be assigned to any computer on an Ethernet network using iSCSI, giving users a vendor-neutral IP SAN instantly.

WINDOWS SOLUTION

---
The first thing you will need to do is perform a basic Windows installation onto your computer. The server I am using is an EOL Dell PowerEdge 1750. The machine that you decide to perform this installation on must have at least two network cards. There is nothing fancy you need to worry about for the installation but the MySAN software required Windows Server 2003 SP1 and .net 2.

I had no success getting this software to install on a Windows 2003 R2 Server with SP2 and .net 2 installed. If you have access to the software like AdminStudio you could potentially modify the InstallShield installer to not perform this check as I’m pretty certain it doesn’t matter.

The following list is a set of steps you can follow to successfully install the pre-req’s for the MySAN software:

* Install Windows Server 2003
* Install Windows Installer 3.0
* Install .net 2.0 Redist
* Install Windows Server 2003 SP1

Obtain the MySAN software from the vendor as well as the license key. The registration is free and provides access to a portal that provides links to both the software and key.

* Install Nimbus MySAN

LINUX SOLUTION

---
The process in Linux is significantly less complicated, any Linux distro can do this just how you obtain the iSCSITarget software will differ based on your preference. For this I’m using Ubuntu 8.04 LTS Server edition so the steps will work perfectly well on any of the Ubuntu versions supported.

The first thing you will need to do is configure a basic server install for Ubuntu, there is no special requirements for either hardware or software. The lone exception is once the install is complete you will need to perform an update and then install the iSCSITarget software. Perform the following tasks after the intial installation is complete:

* sudo apt-get updates
* sudo apt-get upgrade
* sudo apt-get install iscsitarget

Installation is complete at this point and all that is left is configuring the target, you can use either a loopback file or actual media. The benefit of using a file is the ability to run a cron job that would run once a day that would check the utulization of the file and expand as needed.

CONFIGURING THE LINUX ISCSITARGET

---
There is only one file that needs to be modified on the server, /etc/ietf.conf. This file contains the settings for the iSCSI target software and there are a few things that you define here. You will need to set the target name and the path for the disk you are sharing out.

You will want to decide if you want to share the entire filesystem or if you want to share out a file. To the client it doesn’t matter, they see a drive with however much space you define. If you decide to share out an entire filesystem like /dev/sdb modify the /etc/ietf.conf file:

``` bash
LUN 0 Path=/dev/sdb, Type=fileio
```

If you decide to share out a file, you will need to create the file first using dd, then export the path to the actual file:

``` bash
dd if=/dev/zero of=templun3 count=0 obs=1 seek=200G
```

You can then edit your /etc/ietf.conf file:

``` bash
LUN 3 Path=/path/to/file/templun3, Type=fileio
```

MYSAN TARGET CONFIGURATION

---
Once the software has been installed you will need to configure it. On the General tab you may need to click refresh to see your network card, otherwise select the network card you wish to use. Under the targets tab you will need to define which drives you want to make available, if no drives appear you may need to add them through Disk Management.

Create a partition using Disk Management, define a drive letter for your storage and format it. MySAN does not support mounting a drive into a folder. Once the drive has been formatted open the Nimbus MySAN application and click the Refresh button. Next you will need to define the target name for the disk, select the disk and click Add, a dialog appears asking for a target name. This will be the name that your clients will see when you configure the iSCSI initiator, I chose “iscsi.san” for my target name.

Once you have defined your network settings, drive settings and provided a name for the drive you wish to share out, the On button should light up on the General tab. Select On and click Ok, this will start the Nimbus MySAN service and make the disk available on the network.

ISCSI INITIATOR CONFIGURATION

---
I will cover the Windows iSCSI initiator as there may be differences between the various implementations but the main things are covered. You will need to provide the MySAN software with the name of the client’s iSCSI initiator. In Windows this can be found in the Control Panel and the iSCSI Initiator applet.

* If you do not see the iSCSI Initiator you can download it from Microsoft for free and install it, no reboot is required.

Open the iSCSI Initiator on the client, the node name is displayed on the General tab. You may need to change the default name generated at install as it may not work with MySAN. I changed mine to iscsi.client, you will need to provide this name to the MySAN software on your server.

On the server under the Hosts tab of MySAN click Add and provide the name of your client. Click on the Targets tab, select the drive you wish to make available to this client and in the Host with Access dropdown select your client and click Ok.

Back on the client open the iSCSI Initiator, click the Discovery tab, enter the IP or DNS of your server and click Ok. On the Targets tab select the newly listed target and click Logon, the status will switch from inactive to Connected. You can optionally decide if you want the multi-pathing, and to automatically restore the connection on boot.

You may need to open Disk Management on the client and import the newly created disk and format it. After that is done the drive is completely usable.

LINKS

---

[Windows Server 2003 Trial](http://technet.microsoft.com/en-us/windowsserver/bb430831.aspx)
[Windows Installer 3.0](http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=5fbc5470-b259-4733-a914-a956122e08e8)
[.NET Framework 2.0 Redist](http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=0856eacb-4362-4b0d-8edd-aab15c5e04f5)
[Windows Server 2003 SP1](http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=22cfc239-337c-4d81-8354-72593b1c1f43)
[Microsoft iSCSI Software Initiator](http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=12cb3c1a-15d6-4585-b385-befd1319f825)
[MySAN Free Registration Site](http://www.nimbusdata.com/skyline/index.php)
[Ubuntu 8.04 LTS](http://www.ubuntu.com/getubuntu/downloading?release=server-lts)
[AdminStudio Professional](http://www.acresso.com/downloads/downloads_4886.htm)
[iSCSI Defined](http://en.wikipedia.org/wiki/ISCSI)
[iSCSI at Microsoft](http://www.microsoft.com/windowsserver2003/technologies/storage/iscsi/default.mspx)
[Nimbus MySAN Press Release](http://www.echannelline.com/usa/story.cfm?item=21125)
