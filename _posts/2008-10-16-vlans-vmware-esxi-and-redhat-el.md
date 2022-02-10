---
layout: post
title:  "VLAN's, VMware ESxi, and Redhat EL"
date:   2008-10-16 11:21:00 -0600
categories: blog
tags: October 2008 KU SOECS
comments: false
---
In environments were more than 4 VLAN’s exist you will have some difficulties getting all of them trunked into your VM’s. The best way seems to be trunking all the VLAN’s into single interface in your VM and then building multiple virtual interfaces within the guest OS.

There are only 4094 VLAN’s allowed so in order to create this properly the interface in the guest OS needs to be set to 4095. Then you can create a virtual interface for each VLAN on your network.

The following information was taken from RedHat support article [3681](http://kbase.redhat.com/faq/FAQ_45_3681.shtm).

Resolution:

When connected to a properly configured network device, your Red Hat Enterprise Linux 3 system can communicate over a network using 802.1q Virtual Local Area Network (VLAN) tagged frames. The necessary kernel module, 8021q, is already available in the 2.4 kernel.
To use tagged traffic exclusively, create additional ifcfg-ethX.Y files, where X is the interface on which you will use the VLAN and Y is the VLAN ID. For example, on a system with one network card (eth0) that needs to talk to two different VLANs with VLAN IDs 10 and 11, you’ll need these files:

``` bash
/etc/sysconfig/network-scripts/ifcfg-eth0
/etc/sysconfig/network-scripts/ifcfg-eth0.10
/etc/sysconfig/network-scripts/ifcfg-eth0.11
```

These files will configure your system to have two virtual Ethernet interfaces called eth0.10 and eth0.11 that use tagged frames for communication to VLANs 10 and 11. To create the configuration files for the virtual tagged interfaces, copy the contents of your original ifcfg-eth0 file to ifcfg-eth0.10 and ifcfg-eth0.11. Then comment out or remove everything in your ifcfg-eth0 file except for

``` bash
DEVICE=eth0
ONBOOT=yes
```

Next, edit the DEVICE= line in the ifcfg-eth0.10 and ifcfg-eth0.11 files so that they read eth0.10 and eth0.11 respectively. Add the line VLAN=yes to both files. Finish configuring these virtual adapters with the correct IP address and subnet mask for each VLAN, or with a BOOTPROTO=dhcp line if addresses are given out via DHCP. Don’t forget to include a default gateway in /etc/sysconfig/network. It’s important to remember that you can only have one default gateway.
Issue the command:

``` bash
# service network restart
```

to complete the process. The VLAN=yes entries cause the network startup scripts to automatically run the vconfig command to add the necessary VLAN entries in /proc/net/vlan for each VLAN tag.
Here are the completed files for a network set up to only transmit tagged frames and with both virtual adapters set to use

``` bash
DHCP:
/etc/sysconfig/network-scripts/ifcfg-eth0


DEVICE=eth0
ONBOOT=yes
/etc/sysconfig/network-scripts/ifcfg-eth0.10


DEVICE=eth0.10
BOOTPROTO=dhcp
HWADDR=XX:XX:XX:XX:XX:XX
ONBOOT=yes
TYPE=Ethernet
VLAN=yes
/etc/sysconfig/network-scripts/ifcfg-eth0.11


DEVICE=eth0.11
BOOTPROTO=dhcp
HWADDR=XX:XX:XX:XX:XX:XX
ONBOOT=yes
TYPE=Ethernet
VLAN=yes
```

If you accidentally created a virtual adapter with the wrong VLAN ID, you may need to use the vconfig command to remove it from the /proc filesystem. Just restarting the network service won’t do that for you. For example, if you accidentally created a virtual adapter called eth0.12, the following command will remove it from /proc/net/vlan:

``` bash
# vconfig rem eth0.12
```
