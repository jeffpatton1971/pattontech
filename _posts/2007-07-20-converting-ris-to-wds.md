---
layout: post
title:  "Converting RIS to WDS"
date:   2007-07-20 09:05:00 -0600
categories: KU SOECS Lab
tags: July 2007
---
Overview:
There are several methods of converting an existing RIS Server to WDS but the easiest seems to be upgrading to Windows Server 2003 SP 2. If SP 2 is already installed all you need do is choose WDS from the Add Windows Components section of Add/Remove Programs.

Scenario:
Existing RIS Server needs to be converted to WDS, Windows Server 2003 installed with no service packs. Active Directory is installed and configured, RIS is authorized in AD, a DNS and DHCP server are available on the network.

Steps:

1. Download and install Windows Server 2003 SP 2
2. After reboot open the “Windows Deployment Services” MMC
3. Choose “Add Server” from the Action menu
4. Select your server
5. With your server selected choose, “Configure Server” from the Action menu
6. Select your remote installation directory
7. Choose the method to which WDS will respond to clients
