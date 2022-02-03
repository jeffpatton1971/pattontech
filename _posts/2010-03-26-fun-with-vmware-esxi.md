---
layout: post
title:  "Fun with Vmware ESXi"
date:   2010-03-26 19:07:00 -0600
categories: blog
tags: March 2010 KU SOECS
---
In the course of rolling out new hardware we encountered some problems with our up-and-running VMware ESXi servers. I won’t go into that now, but I’ll go over the fun things we found out. Most of these are most likely documented already, but I figured I’d put them here that way I have some place to look the next time I need something!

### SSH

Firstly, managing through the client is nice, but sometimes we need more so below are the steps to enable SSH to your ESXi host:

1. ALT+F1 at the ESXi host console
2. Type, “unsupported” no text will display
3. vi /etc/inetd.conf
4. remove the # sign in front of #ssh stream tcp nowait root…
5. cat /var/run/inetd.pid
6. kill -HUP , this is the number returned from step 5

You should now be able to ssh into your ESXi host using puTTY or ssh, or whatever!

### LOGS

One of the most annoying things for us is the lack of logs from ESXi, not to mention that they roll after each reboot, so you lose them. You can configure local and remote syslogging, I will give you the steps for both. My remote syslog server is a Windows 2008 R2 box that I’m playing with, and I’m using [Kiwi SysLog Server](http://www.kiwisyslog.com/kiwi-syslog-server-download) to receive the logs.

### Install Kiwi

1. Agree to the license terms
2. Select to run as a service
3. Use the LocalSystem account
4. Need a license for WebAccess so it’s pointless to check
5. Choose a “Normal” install unless you need to set things up different
6. Select your destination and click Install

### Enable SysLog

1. Connect to your ESXi host from the VMware Client
2. Click the “Summary” tab
3. Browse your local datastore, not it’s name
4. Create a folder called “logs” inside
5. Close the Datastore Browser
6. Click the “Configuration” tab
7. Click “Advanced Settings”
8. Select Syslog
   1. Syslog.Local.DatastorePath = [Name of Datastore] logs/messages.log
   2. Syslog.Remote.Hostname = IP Address of syslog server
9. Click Ok

Connect to your syslog server and you should now see that logs are available. The nice things is that since these logs are store on a different server, when the ESXi host reboots, you don’t lose them. The logs that are stored on the datastore also survive reboots, it’s nice to have both in case your syslog server fails.

### iSCSI

Apparently ESXi is very touchy about iSCSI targets. If something get’s fouled up it seems the only way to clear it out is to reboot. This is not always fun, and while rebooting was the only thing that worked for us, here are some interesting tidbits we found while working with technical support:

``` dos
/etc/vmkiscsi.conf
```

This file contains connection information for iSCSI targets on the host.

``` dos
/var/lib/iscsi/vmkdiscovery
```

This file contains all the iSCSI targets that this host has discovered. We believe this file is auto-gen’d but what we do notice is that old targets persist even though they are not listed in this file. More digging may be required in order to find additional files that may contain outdated iSCSI targets.

``` dos
/var/lib/iscsi/vmkbindings
```

This file contains a listing of all the iSCSI targets that the host is currently bound to. The recomendation from technical support is that this and the preceding file be renamed to try and resolve the iSCSI issue.

``` dos
esxcfg-swiscsi -d
```

This command disables the software iSCSI initiator

``` dos
esxcfg-swiscsi -k
```

This command kills the software iSCSI initiator

``` dos
esxcfg-swiscsi -e
```

This command enables the software iSCSI initiator

``` dos
esxcfg-swiscsi -s
```

This command triggers a rescan on the iSCSI initiator, this is similar to clicking rescan on the storage tab.
