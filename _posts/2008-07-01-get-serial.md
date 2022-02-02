---
layout: post
title:  "Get Serial"
date:   2008-07-01 11:08:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
---
About 99.9% of the computers we manage are Dell computers. Each Dell computer has a unique service tag that can be used to lookup anything from warranty information to the correct drivers. The serial number is stored in the BIOS and can be changed using a Dell supplied utility.

This function, [GetSerial](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/functions/GetSerial.txt), returns the serial number stored in the BIOS of the computer. It can be used in a number of different ways. One way is to tack it onto the end of a the warranty URL from dell and then you can create a nice clickable table of your computers with the ability to see where each is in the warranty status.

This function accepts one parameter:

strComputer
strComputer
This string variable is usually passed from a calling routine. That routine may be working through a .csv file or it may be pulling this data direct from Active Directory.
