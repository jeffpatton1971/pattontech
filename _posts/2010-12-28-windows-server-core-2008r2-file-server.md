---
layout: post
title:  "Windows Server Core 2008 R2 File Server"
date:   2010-12-28 19:37:00 -0600
categories: blog
tags: December 2010 KU SOECS
---
In order to work around an [issue](2010-12-28-sccm-and-windows-2008r2-failover-clustering.md) we are currently experiencing with System Center Configuration Manager and Windows 2008 R2 Failover Clustering, we need to setup a file server. I decided to set this up as a Core server, and only install the File Services that we need. I won’t walk you through the setup options, just the steps to take once you’re into the console.

Rename the computer

``` dos
netdom renamecomputer %computername% /newname:{newComputerName}
shutdown /r
```

Join the domain

``` dos
netdom join ComputerName /domain:DomainName /userd:UserName /passwordd:*
shutdown /r
```

Install the required roles

``` dos
dism /online /enable-feature /featurename:CoreFileServer
dism /online /enable-feature /featurename:NetFx2-ServerCore
dism /online /enable-feature /featurename:FSRM-Infrastructure-Core
```

Configure the firewall

``` dos
Netsh advfirewall firewall set rule group=”File and Printer Sharing” new enable=yes
Netsh advfirewall firewall set rule group=”Remote Volume Management” new enable=yes
Netsh advfirewall firewall set rule group=”Remote Administration” new enable=yes
```

I don’t know if I need to explain a whole lot here, the first two things are fairly self-explanatory. Windows 2008 has a lovely computername after installation so you really want to rename it. Fortunately the %computername% works, so you don’t need to know what it is before you rename it. In order to join the domain you will need to reboot after the rename, if you don’t you will receive an error:

> Failed to access network address

That newly renamed computer doesn’t actually exist yet, seems funny I know, but just do the reboot. You will also need to reboot after joining the domain. Thankfully though, that is the end of the reboots.

You will need to install the CoreFileServer role in order to actually create and share folders on the server. I enjoy using the File Server Resource Manager, in order for that to work you will need to install the next two features, NetFx2 and FSRM, FSRM requires .NET so they need to be done in that order.

Finally in order for clients to access your file shares, and for you to be able to manage them you will need to enable the rule groups listed above.
