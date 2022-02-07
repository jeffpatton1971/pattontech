---
layout: post
title:  "Preparing to upgrade DC's to Windows Server 2008 R2"
date:   2011-06-10 13:08:00 -0600
categories: blog
tags: June 2011 KU SOECS Scripting
---
We decided it was time to move to R2 on the domain controllers, while we’re at it we also moved up to Windows 2008 Functional. Sadly we can’t go to 2008r2 functional until our last 2008 DC goes out of warranty, in 2014! Oh well, maybe something will happen to it…

Fairly straightforward process

1. Transfer FSMO roles
2. Take a VM snapshot
3. Perform the DCPromo
4. Uninstall ADDS binaries
5. Export Logs
6. Reset computer account
7. Install Windows 2008R2 Core

Well this morning [Boe Prox](http://learn-powershell.net/) posted a nice function to [poshcode](http://poshcode.org/), [Get-FSMORoleOwner](http://poshcode.org/2727). This function returns what server owns which roles and it was very helpful as I performed my transfers. Originally I had wanted to use Powershell to perform my transfers, but without [ADWS](http://technet.microsoft.com/en-us/library/dd391908(v=ws.10).aspx) the built-in [ActiveDirectory module](http://technet.microsoft.com/en-us/library/ee617195.aspx) wouldn’t load. So I performed the transfers the old-fashioned way, using the MMC. After each transfer I would run Get-FSMORoleOwner to verify that the role had been moved. This all went without a hitch as expected, then came the Domain Functional Level.

No big surprises here, we did a little digging and found that moving to 2008 Functional level was equivalent to staying at 2003. There are a few new features in 2008, the most interesting one for me at least, is the Interactive Logging. Raising the functional level was a breeze, and all domain controllers reported the same level within minutes.

Since I’ve been moving away from VBscript and over to PowerShell I decided to take my VM snapshot using the [PowerShellCLI](http://communities.vmware.com/community/vmtn/server/vsphere/automationtools/powercli) from VMware.

``` powershell
New-Snapshot -Name Pre-Demotion -VM dc1 -Description “Snapshot prior to demoting.”
```

In order to backup all the logfiles I threw together a quick little function [Backup-EventLogs](https://github.com/mod-posh/ComputerManagement#backup-eventlog). It takes the name of the computer and uses [Get-WinEvent](http://technet.microsoft.com/en-us/library/dd367894.aspx) to get all the logs available. It then writes out each log where the RecordCount is greater than 0 to a csv file, using [Export-CSV](http://technet.microsoft.com/en-us/library/dd347724.aspx). Carson pointed out that it may have been easier to copy the log files over…ya, well…dammit I wouldn’t have gotten a nifty function out of it though!

   Anyway, resetting and re-installing are fairly vanilla, and I’m pretty sure I covered standing up a core server [somewhere]({% post_url 2010-05-17-dpm-2k7-w2k8r2-w2k8-core-dc ) before.

Thanks,
