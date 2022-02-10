---
layout: post
title:  "PowerShell New-AdInventory script"
date:   2012-03-20 15:08:00 -0600
categories: blog
tags: March 2012 KU SOECS Scripting
comments: false
---
I may have mentioned on here before that we rely quite heavily on Active Directory, and it’s true. It’s at the core of nearly all the services we deliver, the only exception would be the web, and that would really only be the public facing web sites.

I’ve also mentioned before that I’ve been moving over from VbScript to PowerShell, and I think it’s safe to say that I moved over quite a while ago. If you’ve not browsed my scripts you should head over to my [code.google.com](https://github.com/jeffpatton1971/mod-posh) site to see what I’ve done.

Anyway, today I was working on a problem with a script that runs from a cron and after fixing that one, I realized I was still using my old [inventory](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/ADInventory.vbs) script to update Active Directory computer objects with some useful information. So I decided it was time that I rolled this script over to PowerShell. Now while I’d like to say the new and improved one is much more wicked awesome, it’s not, it’s just all PowerShell’d up.

The previous script I had created several functions to do things like send data to the event log. A rather generic function to return values from a remote computer via WMI. A nice little function to ping the computer, although looking back at the code I noticed that it’s not actually there, I should fix that.

At any rate the new [script](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/New-AdInventory.ps1) seems to go a little faster, and it certainly doesn’t look any shorter but most of that is actually documentation. Although technically since I [dot-source](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/includes/ActiveDirectoryManagement.psm1) in a library it’s significantly larger than the previous script.

This runs every hour and pulls the UserName, MacAddress, IPAddress and SerialNumber from the remote computer via WMI. I then write these values back to the computer object more or less using the same properties. Although description becomes UserName and ipHostNumber becomes IPAddress.

The nice thing is that we can then visually scan a given OU and see who might be logged into a computer. If there is an issue connecting to a computer, that is also written to the description property. That way as you browse your AD you can easily see which computers have problems, typically these are also dead computer accounts.

The code is also available on [Technet](http://gallery.technet.microsoft.com/New-AdInventoryps1-2b1bd7e2)(Gallery is no longer active).
