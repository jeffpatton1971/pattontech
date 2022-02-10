---
layout: post
title:  "Get Username"
date:   2008-07-01 11:04:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
comments: false
---
One of the things we find handy is the ability to know who is logged on to a given computer at any one time. The main reason is that our users rarely tell us which computer they are on when they submit a help-request, so if we have someplace to look that makes our jobs that much easier.

The [Getuser](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/functions/GetUser.txt) function returns the name of the currently logged on user, or the word “FREE” if nobody is logged on. I use this in conjunction with another function that updates the Active Directory description property of the computer object.

This function accepts one parameter:

strComputer
strComputer
This string variable is usually passed from a calling routine. That routine may be working through a .csv file or it may be pulling this data direct from Active Directory.
