---
layout: post
title:  "Group Policy Preferences in a mixed environment"
date:   2010-01-19 19:02:00 -0600
categories: blog
tags: January 2010 KU SOECS Scripting
comments: false
---
We were recently directed to push Windows 7 out to the network which triggered the previous [post]({% post_url 2009-11-30-windows-7-deployment-part-1-testing %}) on Windows 7 deployment. In order to make this happen we made a significant investment in hardware. The targeted group of computers, approximately 150, have been upgraded to 4gb of ram and at least a 150gb hard drive.

With the introduction of UAC in Windows Vista it is no longer possible to allow a regular user to install print drivers or any driver. So you have to decide on how you want to manage your users, do you want them to be admins and let UAC do it’s job, or not. For the time being we have opted to not let them be admins. Which means we’ve had to find different ways to make things work, so we’re looking to [GPO Preferences](http://technet.microsoft.com/en-us/library/cc731892(WS.10).aspx) to solve some of our issues.

I have to admit it’s been rather frustrating trying to map printers and drives after Windows Vista. I wrote a pretty awesome [logon script](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/UnifiedLogonScript.vbs) that would map any object in AD with a UNCPath property. In order for us to have a single printer in more than one place in AD we opted to use the [Shared Folder](http://technet.microsoft.com/en-us/library/cc781526(WS.10).aspx) object for printers and, well shared folders. This method worked for a long time under Windows XP, but as we started bringing Windows Vista into the fold we had to change our thinking.

With Preferences you are able to do a great many things, but in testing they don’t appear to work as advertised. We are having limited success with drive and printer mappings. So we took a mixed bag approach, we are able to deliver drive mappings using GPO Preferences but the only luck we’ve had with printers is using the [Deploy Printer](http://technet.microsoft.com/en-us/magazine/2006.08.manageprinters.aspx) method from our Windows Server 2003 R2 print server.

The only issues with that method of deployed printers is if you routinely move workstations around, the printer connections can get tattooed to your client’s registry. It’s not that big of a deal as you can write a startup script that nukes that portion of the registry.
