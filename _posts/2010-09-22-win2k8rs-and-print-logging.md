---
layout: post
title:  "Windows Server 2008 R2 and Print Logging"
date:   2010-09-22 19:28:00 -0600
categories: blog
tags: September 2010 KU SOECS
comments: false
---
Yesterday I got a support ticket that stated the user was unable to connect to any printers on the network. I found this odd since we push out printers via GPO Preferences, and that user’s particular computer had the correct GPO linked in. So I started my troubleshooting process which usually involves grep’ing the logs looking for relevant entries.

We have two print servers, one is running Windows Server 2003 R2 and the other is running Windows Server 2008 R2. Both have about 40 odd printers installed, and that is their sole purpose in life. One of the things we realized a long time ago is that the event logs for Windows printing is really horrible. So we have downloaded and installed the [PaperCut Print Logger](http://www.papercut.com/products/free_software/print_logger/) software.

I noticed that he had connected to the print server earlier that morning to print, but nothing since. So I decided to check in the system log, which is where I would go on Windows Server 2003. I was surprised to see there were no entries related to printing at all, then I noticed there was a view for Roles, and that Print and Document Services was an option to view.

When I looked through that log, I noticed that the only events in it were related to installing printers and their associated drivers. So opening up the Applications and Services Logs folder, expanding the Microsoft folder, expanding the Windows folder,  and scrolling down to PrintServices I found an Operational log, that had nothing in it. When I opened that log up I noticed that from the list of Actions I could Enable Log, once I did that I started seeing entries show up.

It turns out the default setting for this log is off, even if you pick the File and Print server role which seems a bit counter-intuitive to me. So remember, when setting up your print server on Windows Server 2008, to enable logging perform the following steps:

1. Login to the server
2. From Server Manager, expand Diagnostics and Event Viewer
3. Expand Applications and Services Logs
4. Expand the Microsoft folder, expand the Windows folder
5. Expand the PrintServices folder

Right-click on the Operational Log, and click Enable Log
