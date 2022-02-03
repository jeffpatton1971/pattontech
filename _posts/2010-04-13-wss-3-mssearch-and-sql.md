---
layout: post
title:  "WSS 3.0, MSSearch Service and SQL"
date:   2010-04-13 19:09:00 -0600
categories: blog
tags: April 2010 KU SOECS
---
In my previous [post](2010-04-13-sql-server-upgrade.md) I talked about the SQL upgrade that we performed recently. I left out the part that had to do with our SharePoint server. While it is fundamentally related I thought I would share some of the dead-ends I ran in to.

About a [year](2009-04-02-sharepoint-upgrade.md) ago we replaced our existing SharePoint server with one that provided a little more flexibility. We created a simple farm, with a web front-end and SQL Server backend. This ran quite well for us up until we upgraded hardware on the SQL Server. Again a little better preparation beforehand then a lot of these problems would not have occurred.

Search on the SharePoint site had been more or less broken for a while, and we were waiting for this recent upgrade to attempt to get it working properly again. I can’t really tell you what caused it to get balled up, other than we may have reset permissions on an early SAN migration. At any rate, search would return what you wanted sometimes, and sometimes it didn’t.

Below is a list of the error messages that I ran into as I worked through the problem. After understanding the fix in my case, and seeing them all laid out it’s obvious that SharePoint just couldn’t get to the SQL server. Adding the sqlsrvr.exe application to the list of allowed programs through the firewall is what resolved the issue for us. So the proper course of action would have been to set the firewall rule first, and then stop and restart the search service.

It sounds rather scary, when you first click the stop link in the SharePoint Central Admin website it says it’s going to remove everything. If your search is broken, that is what you want. If you’re very nervous you can simply create a new database for search, you will want to look at this [article](http://technet.microsoft.com/en-us/library/cc288606.aspx) from Microsoft. Once you start the service back up it will create all the needed tables and stored procedures.

Error messages encountered

> “Your search cannot be completed because of a service error”
> “Could not access the Search service configuration database”
> “The search request was unable to connect to the Search Service.”
> “retry of query machine has failed with error the system cannot find the file specified”
