---
layout: post
title:  "Windows Server 8 Beta Failover Clustering Part 2"
date:   2012-03-20 15:06:00 -0600
categories: blog
tags: March 2012 KU SOECS MissingImages
comments: false
---
There were so many images that I decided to split this up over several posts. In [Part 1]({% post_url 2012-03-20-windows-server-8-beta-failover-clustering-part1 %}), I got the Failover Cluster feature installed on my first server, and now I’m going to work through the Failover Cluster Management tool.

![win8-hv1-FC1]()

So, the interface doesn’t look any different. I was hoping for something that was part of the Dashboard.

![win8-hv1-FC-wizard1]()

So here is the wizard, it’s really almost identical to Windows 2008 R2, but what the heck.

![win8-hv1-FC-wizard2]()

I’ve selected both servers to be nodes in my new cluster.

![win8-hv1-FC-wizard3]()

Oh wait! I forgot to install the Failover Cluster feature on win8-hv2!

![win8-hv1-AddFeatureRemote]()

No worries, I got it covered, I can add that feature remotely from the dashboard. That option has to be one of the coolest one’s I’ve seen.

![win8-hv1-FC-wizard4]()

Needless to say the feature installed successfully and I’m able to proceed now.

![win8-hv1-FC-wizard5]()

None of the hardware I’m using is technically support by Microsoft but it’s Beta software so who cares right? Let’s see what the report says though.

![win8-hv1-FC-validation-wizard1]()

Welcome to the Wizards, is it just me or are there more of them?

![win8-hv1-FC-validation-wizard2]()

Lets run everything, I know it will have issues, as both machines are different and don’t have the same sets of software available.

![win8-hv1-FC-validation-wizard3]()

Here we go…

![win8-hv1-FC-validation-wizard4]()

That looks good!

![win8-hv1-FC-validation-wizard5]()

OUCH! Well, win8-hv2 doesn’t actually have Hyper-V installed and since that was in the test, that’s where it failed.

![win8-hv1-FC-validation-report1]()

The report confirms this. But again, thanks to being able to remotely install Roles and Features, I installed Hyper-V on the other server and re-ran the tests.

![win8-hv1-FC-validation-report2]()

Much happier! For the record, there are several warnings.

* Hyper-V : The processors are different between the two machines.
* Network : I don’t have redundant network cards
* Storage : I don’t have any storage available suitable for some types of clustering, which could be an issue for Hyper-V (Clustered Shared Volumes)
* System Configuration : To be honest, I didn’t actually look at this, I was aware of the others so I assume there is something minor missing or different between the two nodes.

![win8-hv1-FC-wizard6]()

Here we go, this is good enough to move on to create the end-point for the cluster. This is one of the ways you’ll be able to manage it.

![win8-hv1-FC-wizard7]()

Here it is saying it’s going to steal some IP’s from my range

![win8-hv1-FC-wizard8]()

My cluster is forming!

![win8-hv1-FC-wizard9]()

I was successful! But there were warnings, basically it’s telling me I don’t have any sort of network storage to use for this cluster, of which we were already aware.

![win8-hv1-FC2]()

Huzzah! The one warning was from the cluster I built earlier with just the one node. Perhaps should have nuked the cluster log, oh well.
