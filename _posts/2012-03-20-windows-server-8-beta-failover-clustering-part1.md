---
layout: post
title:  "Windows Server 8 Beta Failover Clustering Part 1"
date:   2012-03-20 15:05:00 -0600
categories: blog
tags: March 2012 KU SOECS MissingImages
---
I read a very nice [article](http://blogs.msdn.com/b/clustering/archive/2012/03/19/10285168.aspx) over on TechNet about some of the new features of Failover Clustering in Windows 8, so I decided to give it a spin. It’s not too bad, I’m really enjoying the new Server Manager interface, it’s pretty awesome.

Here are the screenshots of the setup process.

![win8-hv1-step1]()

Here is the dashboard, this particular computer was upgraded to Windows 8 Server from Windows 2008 R2, so there were already a few roles installed.

![win8-hv1-wizard1]()

Similar interface for adding the Failover Clustering Role

![win8-hv1-wizard2]()

I’ve not tried the Remote Desktop VDI stuff yet, I’m thinking I may grab a couple of more machines and start over with fresh installs.

![win8-hv1-wizard3]()

Here you can install roles onto one of the servers in your pool. I added win8-hv2.soecs.ku.edu to the pool so I could manage both servers from one interface, cool huh?

There is also the option to install the role to VHD, I will have to try that later!

![win8-hv1-wizard4]()

Here you can see the installed roles from Windows 2008 R2

![win8-hv1-wizard5]()

There’s the feature I’m after, Failover Clustering. Note the additional tools, there are 81 PowerShell cmdlets available for managing Failover Clustering. I’m going to post those up after this.

![win8-hv1-wizard6]()

Here is everything that will be installed, I checked the box to Restart the destination if necessary, but for this feature it’s not necessary. But as I was messing around with various other components earlier this week, that’s a nice option.

![win8-hv1-wizard7]()

The installation is starting, you can see the notification flag now has a 1 inside it’s little box. You can close this window and the install will progress.

![win8-hv1-TaskStatus]()

Clicking on the notification flag, you can see all tasks that are currently running.

![win8-hv1-TaskDetails]()

Here is what you see when you click details. By the time I got to this screen the installation was done.

![win8-hv1-Manage1]()

I can manage the Cluster from the Dashboard | Tools menu

![win8-hv1-Manage2]()

Since it’s installed on win8-hv1 I can right click on that server in the Server Pool and select Failover Cluster Manager from there.

![win8-hv1-Manage3]()

Perhaps this is silly to point out, but the Failover Clustering feature was not installed on win8-hv2 and so you don’t see the option to manage it from there.
