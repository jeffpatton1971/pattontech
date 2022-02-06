---
layout: post
title:  "RedHat 6 Enterprise + Vmware Vsphere"
date:   2012-04-01 15:16:00 -0600
categories: blog
tags: April 2012 KU SOECS MissingImages
---
Odd thing happened today while I was setting up a server for someone. The RHEL 6 install went just fine, it found the network card, asked if I wanted to configure it via DHCP and all that. When the server rebooted, there was no eth0. When I ran ifconfig eth0 up, the adapter showed up, but then wouldn’t get an IP from DHCP. A quick look at the messages showed the following

![rhel6-nonic]()

No broadcast interfaces found?

Unhandled state?

Now I admit that I’m not much of a Linux admin, I have setup and done some cool stuff, but it’s just not my forte. So I resorted to Google, when Nick and Carson didn’t the answer for me!

I found this [thread](http://forums.fedoraforum.org/showthread.php?t=252210). In it the user stated that if he ran dhclient eth0 the adapter would go, so I did that and sure enough I got an IP address. When I started poking around I noted that in /etc/sysconfig/network-scripts/ifcfg-eth0 that ONBOOT was set to NO, I changed this to YES. Then to verify that it was working properly, I stopped the network service, shutdown the computer, and moved it to a different VLAN.

When the server came back up, it had an IP address, I’ve never had RHEL do that before, and I’ve done several installs, so I guess it’s a fluke? But if not, at least I’ve documented how I got it to go if it happens to me again!
