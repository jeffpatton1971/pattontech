---
layout: post
title:  "Get MAC Address"
date:   2008-07-01 11:06:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
---
In managing our IP space we rely heavily on the MAC address of a computer. This seems to be slightly backwards of how most people manage their IP space, but we consider the IP to be a property of the MAC, and the MAC itself represents the computer.

The [GetMac](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/functions/GetMac.txt), function grabs the MAC address of the network interface of the computer. In Windows there are multiple “interfaces” so if the interface doesn’t have an IP address in the proper range I don’t worry about it. We have noticed that on some machines, apparently randomly, the correct IP and MAC are reported back twice, so I exit the entire loop once I get a MAC address.

This function accepts one parameter:

strComputer
strComputer
This string variable is usually passed from a calling routine. That routine may be working through a .csv file or it may be pulling this data direct from Active Directory.
