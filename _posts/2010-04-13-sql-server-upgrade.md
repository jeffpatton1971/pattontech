---
layout: post
title:  "SQL Server Upgrade"
date:   2010-04-13 19:08:00 -0600
categories: blog
tags: April 2010 KU SOECS
---
In an earlier [post]({% post_url 2010-03-24-windows-2008rs-clustering %}) I mentioned that we were in the process of upgrading hardware and moving something’s around on the back-end. The end-result of this process would make us feature complete in our stated goals. Well, we had a few hiccups along the way, hopefully in the future I’ll do better at testing all this out. One of the things that needed to happen was our existing SQL server needed to be replaced with something that had significantly more power.

The new SQL server is a Dell PowerEdge R310 with 16GB of Ram and dual Quad core Xeon’s, this is a vast improvement over the hardware replaced. In my own personal goal to keep us as current as possible it was loaded with Windows 2008 R2. We would have liked to upgrade to SQL 2008, but support for that is not quite there yet with System Center Configuration Manager. So we loaded SQL Server 2005, and before you could do anything the most recent service pack needed to be applied.

The old server ran Windows Server 2003 R2 and we had never configured the firewall. Since 2008 comes with the firewall turned on, we opted to make it go with the firewall. If we only had a single instance it would have been much simpler, but we have 4 named instances that needed to work properly. After bringing our first instance online and restoring the data, we found that the best thing to do was to add the program binary to the list of allowed programs.

We performed the following steps for each of the named instances:

1. Start > Control Panel > Windows Firewall
2. Allow a program or feature through Windows Firewall
3. Change Settings
4. Add another program
5. browse to the instance bin folder for the sqlsrvr.exe

Once that was done we were able to successfully connect to SQL and get services started.
