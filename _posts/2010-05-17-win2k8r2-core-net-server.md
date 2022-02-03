---
layout: post
title:  "Windows Server 2008 R2 Core Net Server"
date:   2010-05-17 19:12:00 -0600
categories: blog
tags: May 2010 KU SOECS
---
I wanted to setup a DNS and DHCP server for my home network. I could have used a Linux server to do this, but I wanted to write about server core. I also wanted to mess with working over ssh to do things, I found a SourceForge [project](http://opensshwindows.sourceforge.net/) that worked really well on 2008 x86, I had errors on 2008 R2 which I’ll poke at next week.

I don’t think I need to explain these things too much, I’ll be using DNS and DHCP as they were intended to be used. I’m installing the SNMP service because I have setup a Zenoss server. It uses SNMP to monitor everything from disk usage to network usage. You will need to have your network card set to a static IP in the proper network for it work correctly.

PowerShell Installation

Windows 2008 Core

Install PowerShell following these [instrcuctions](http://dmitrysotnikov.wordpress.com/2008/05/15/powershell-on-server-core/).

Windows 2008 R2 Core

Install PowerShell following these [instrcuctions](http://www.betteritsolutions.co.uk/?p=78).

Install OpenSSH

* Download the install [file](http://sourceforge.net/projects/opensshwindows/)
* The installation takes care of everything but the firewall
* Allow SSH through the firewall

``` dos
netsh firewall set portopening TCP 22 “OpenSSH TCP22”
```

Basic Networking Configuration

``` dos
netsh interface set interface name=”Local Area Connection” newname=”Public”
netsh interface ipv4 set address name=”Public” static 192.168.1.1 255.255.255.0 192.168.1.254
netsh interface ipv4 set dnsserver name=”Public” static 192.168.1.1 primary
netdom renamecomputer %computername% /newname:{newComputerName}
```

Installing the required services

DNS

``` dos
start /w ocsetup DNS-Server-Core-Role
```

DHCP

``` dos
start /w ocsetup DHCPServerCore
```

SNMP

``` dns
start /w ocsetup SNMP-SC
```

Configuration of these services can be done via a server with RSAT installed, but that would be no fun at all, so we’ll use the builtin tools to set things up. If you have read the above article you may already have SSH setup and be using puTTY or some other client to perform these tasks.

Configuring the required services

DNS: dnscmd

The following command creates a forward lookup zone and marks it as the primary zone on this server.

``` dos
dnscmd ServerName /ZoneAdd company.com /Primary /file company.com.dns
```

The following command creates a reverse lookup zone for 192.168.1.0/24 and marks it as the primary zone

``` dos
dnscmd ServerName /ZoneAdd 1.168.192.in-addr.arpa /Primary /file 1.168.192.in-addr.arpa.dns
```

The following command creates an A record for the local server at 192.168.1.1

``` dos
dnscmd ServerName /RecordAdd company.com @ A 192.168.1.1
```

The following command creates the reverse record for the server at 192.168.1.1

``` dos
dnscmd ServerName /RecordAdd 1.168.192.in-addr.arpa 1 PTR company.com
```

Allow DNS traffic to pass through the firewall

``` dos
netsh firewall set portopening TCP 53 “DNS-TCP”
netsh firewall set portopening UDP 53 “DNS-UDP”
```

DNSCMD Resources:

[Setup DNS from the command-line](http://www.windowsreference.com/dns/how-to-setup-and-configure-dns-from-command-line-in-windows-server-2008-core/)

[Configuring DNS on Server Core](http://jefferyland.wordpress.com/2009/03/18/configuring-dns-zones-in-core/)

[DNSCMD Cheat Sheet](http://thebackroomtech.com/2008/03/24/marks-windows-2008-dns-server-command-line-cheat-sheet/)

[Administering DNS Server](http://technet.microsoft.com/en-us/library/cc794771(WS.10).aspx)

DHCP: netsh

After installing the DHCP role on the core server you will need to run the following command to allow the service to auto-start

``` dos
sc config dhcpserver start= auto
```
