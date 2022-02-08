---
layout: post
title:  "'Single node cluster' oxymoron or reality?"
date:   2010-10-05 19:32:00 -0600
categories: blog
tags: October 2010 KU SOECS
---
You might be asking yourself, what? how? why? To be honest I asked myself the very same questions. I don’t purport to have all the answers to these important questions, but I have found a few resources, and I wrote a little step-by-step to show answer the how.

What is a “single node” cluster? Well, it’s technically not a cluster at all, but it provides the same sets of services an actual cluster would. It’s a way to have different views of the same server, Microsoft refers to these as “virtual servers”. To simply, a virtual server in this sense is simply a hostname associated with a specific IP address on your cluster.

Why would I want a “single node” cluster?  That is a really good question, why would you want a cluster that provided none of the wonderful things that clusters provide; failover, redundancy, fault-tolerance, high-availability. These are really all saying the same thing, the single most important thing a cluster provides is failover. When one node fails, those services roll over to the next available node. On a “single node”, there isn’t another available node to fail over to.

So really, what is the point? Well, the example I found on the [TechNet](http://technet.microsoft.com/en-us/library/cc780872(WS.10).aspx) site was very simple. You have a single server in your office, but for administrative reasons, you want each department to access their “own server”. So you create Resource groups for each that contain at a minimum an IP Address resource and a Network Name resource.

Now you might be asking yourself, why did he decide to write an article about this? Well, it turns out that we are in the middle of testing an IPAM solution. We needed to be able to test how our existing cluster would behave, so we created a vm, installed Windows and setup a cluster, that we could use to duplicate our current infrastructure, without having to setup anything crazy.

How do I setup a “single node” Windows Server 2003 cluster? ([Based on this TechNet article](http://technet.microsoft.com/en-us/library/aa996134(EXCHG.65).aspx))

![20101005-Cluster2003Step1](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2003Step1.png)
![20101005-Cluster2003Step2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2003Step2.png)
![20101005-Cluster2003Step3](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2003Step3.png)
![20101005-Cluster2003Step4](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2003Step4.png)
![20101005-Cluster2003Step5](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2003Step5.png)
![20101005-Cluster2003Step6](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2003Step6.png)

Configuring a Windows Server 2008 “Single node” cluster.

![20101005-Cluster2008Step1](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2008Step1.png)
![20101005-Cluster2008Step2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2008Step2.png)
![20101005-Cluster2008Step3](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2008Step3.png)
![20101005-Cluster2008Step4](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2008Step4.png)
![20101005-Cluster2008Step5](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2008Step5.png)
![20101005-Cluster2008Step6](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2008Step6.png)
![20101005-Cluster2008Step8](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2008Step8.png)
![20101005-ClusterSuccess](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-ClusterSuccess.png)

If you have a third party DNS Server, you may see this after your cluster is done.

![20101005-Cluster2008BadDNSPacket](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101005-Cluster2008BadDNSPacket.png)

I wrote an [article]({% post_url 2010-03-24-windows-2008r2-clustering %}) a while ago about how to resolve this issue. But there is also a good [article](http://social.technet.microsoft.com/wiki/contents/articles/event-id-1196-microsoft-windows-failoverclustering.aspx) available on the [Technet Wiki](http://technet.microsoft.com/wiki).
