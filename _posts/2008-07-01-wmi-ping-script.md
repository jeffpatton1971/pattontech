---
layout: post
title:  "WMI Ping Script"
date:   2008-07-01 11:07:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
---
Most of the computer management scripts run against multiple computers, I usually want to have the script do one task if the computer is available for management or another when it’s not. So I put together a very simple function, [WMIPing](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/functions/WMIPing.txt), that takes advantage of the Err object.

The function accepts a single value:

strComputer
strComputer
This string variable is usually passed from a calling routine. That routine may be working through a .csv file or it may be pulling this data direct from Active Directory. I attempt a connection to the computer, if it fails I set the online flag to False, otherwise I set it to true.
