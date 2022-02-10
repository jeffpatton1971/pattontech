---
layout: post
title:  "Windows 2008 R2 Clustering"
date:   2010-03-24 19:06:00 -0600
categories: blog
tags: March 2010 KU SOECS
comments: false
---
Our current file server cluster falls out of warranty in about a month, and as it was over-spec’d to begin with we made a purchase of new hardware from Dell. We bought several things, two new servers for the file sharing cluster, a new sql server, a new backup server, a new virtual server, and two new iSCSI sans. I’ll talk about these as we get to them, for the time being I’ll talk about Failover clustering in Windows 200r R2.

I started working on a post for setting up a Hyper-V cluster, but I’ve not gotten around to getting that done so I’ll use this as a starting point. I won’t go in to any great detail of how to install things, the installation is rather straightforward. I will mention a few things first though:

* Failover clustering feature is only available on Windows 2008 Enterprise or above
* If you’re running an iSCSI SAN enable the MultiPath IO Feature
* Do yourself a favor and enable the SNMP Feature

Considerations for File server clusters

* Enable the File Server role, if you’re setting up a file server cluster
* Enable the [Remote Volume Management](http://technet.microsoft.com/en-us/library/cc771775.aspx) rule in the Windows Firewall

Once everything is installed and ready to go you can run the, “Cluster validation report” and it will tell you what, if anything, you need to resolve prior to getting the cluster up and running. If you happen to be running a non-Microsoft DNS server (BIND) then the following information will be important for you. Once we had our cluster setup we noticed repeated error’s for Event ID 1196. After googling for it I found a nice blog [post](http://blogs.msdn.com/clustering/archive/2009/07/17/9836756.aspx) related to how DNS works for Windows Clusters, which specifically talked about using a Microsoft DNS server. After a little more googling I found [KB 977158](http://support.microsoft.com/kb/977158). This article directly related to our situation and upon applying the HotFix mentioned our cluster was able to finally come online. You can read the details in the article itself on what the problem was, but if you are running BIND for name resolution and want a Windows 2008 R2 cluster, install the hotfix.

When setting up a file server cluster in Windows 2003, you were provided a very minimal amount of configuration. You would want to create the folder in advance, define your NTFS permissions, if you were running Storage Server or R2 you could then define your folder based quotas and then, and only then could you create your File Share resource on the cluster.

The first thing you will want to do is make sure that you have disks available for storage, then create your first File Server resource. Once you have your File Server resource online you right-click to “Add a shared folder”. From this point on it provides a wizard that guides you through the things you need to setup. Below are screenshots that step you through each screen. Once you click finish it sets everything you want up prior to making the share available on the cluster! You no longer have to visit multiple consoles, it’s all literally at your finger tips.

I know I get excited about many things, but I know for me, this will make things so much simpler!
