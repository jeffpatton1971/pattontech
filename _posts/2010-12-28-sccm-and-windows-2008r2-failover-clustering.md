---
layout: post
title:  "SCCM and Windows 2008 R2 Failover Clustering"
date:   2010-12-28 19:36:00 -0600
categories: blog
tags: December 2010 KU SOECS
comments: false
---
So yesterday we were able to migrate our [existing]({% post_url 2010-03-24-windows-2008r2-clustering %}) Windows Storage Server 2003 R2 file cluster to the Windows 2008 equivalent of the same. It has been a long time in the works as we had to wait on Dell to have a Windows Storage Server 2008 certified [device]({% post_url 2010-03-26-fun-with-vmware-esxi %}). Our only hitch was forgetting to down the resource groups on the old server prior to bringing the lun’s over to the new server. Once we got that worked out, things went smoothly and once all the drives were moved, I was able to get all the shares set back up and we were back in business before lunch.

Sadly, once the move was complete our existing SCCM server started to complain.

> SMS Site System Status Summarizer could not access storage object “\fsdistro_test” on site system “\fsdistro_test”. The operating system reported error 2147942467: The network name cannot be found.

While this sounds like a network issue and looks like a network issue we believe it has very little to do with networking. The server listed above can be pinged from the SCCM server and we can browse to that share. We think that what’s actually going on is that SCCM is unable to connect to any of the [administrative shares](http://en.wikipedia.org/wiki/Administrative_share) on that server, which makes sense since those don’t exist on that server.

This all goes back to how Windows 2003 handled clustering, when you created a file server resource you gave it a name and IP. This worked in a fashion very similar to a [CNAME](http://en.wikipedia.org/wiki/CNAME_record) in DNS. Whatever server in your cluster was currently hosting that resource would respond to the request. Which also meant that any underlying file shares would also be available from that file server resource. So let’s say you have a file server resource called FS, and your cluster server is Node1.

Under Windows 2003 Clustering the following resources would all be available when you browsed to either Node1 or FS1.

* \node1admin$
* \fsdistro_test
* \node1files

Under Windows 2008 Failover clustering when you browse to Node1 you would see:

* \node1admin$
* \node1files

And when you browsed over to FS you would see:

* \fsdistro_test

Windows 2008 Failover clustering does a much better job at handling those file server resources. They look and feel almost like separate file servers, but that isn’t enough for System Center. SCCM needs to be able to access the admin$ share on FS, but since that doesn’t exist the Distribution Point fails, and we are unable to push update software to our clients.

The best solution we could come up with, until hopefully someone at Microsoft can give us a better answer is to stand up a separate server. Our only issue with this is, it’s yet another server to manage, secure, and keep up to date.
