---
layout: post
title:  "Hyper-V Server 2012 Cluster with Powershell Deployment Toolkit"
date:   2013-05-31 11:02:00 -0600
categories: blog
tags: May 2013 KU IT
---
I recently came across a lovely [show](http://channel9.msdn.com/Shows/Edge/Edge-Show-62-Hyper-V-Server-2012-Cluster-Deployment-Using-Free-Tools) on [Channel 9](http://channel9.msdn.com/). It talks about setting up a simple [Hyper-V Server 2012](http://www.microsoft.com/en-us/server-cloud/hyper-v-server/default.aspx) cluster for use in a lab or test environment or whatever. I won’t go over the details, watch the show, it’s great! In addition to that I had come across an [article](http://blogs.technet.com/b/privatecloud/archive/2013/02/08/deployment-introducing-powershell-deployment-toolkit.aspx) on the [Building Clouds Blog](http://blogs.technet.com/b/privatecloud/), about the [PowerShell Deployment Toolkit](http://gallery.technet.microsoft.com/PowerShell-Deployment-797b3c6d). So over Memorial Day weekend I decided to stand up my cluster and spin up a test environment similar to what I use at work.

In my environment I have 6 servers, I have 3 set aside for Hyper-V, one is my firewall, one is a Domain Controller and the last is a management server. I’m using my DC as the file server as well. I didn’t need the iscsi target stuff, as I’m using Windows Server 2012 and used the new [File and Storage Services](http://technet.microsoft.com/en-us/library/hh831487.aspx) to configure my iscsi drives.

I decided to let [vmcreator.ps1](http://blogs.technet.com/b/privatecloud/archive/2013/02/18/deployment-the-pdt-vm-creator.aspx) build the vm’s for me, originally I had spun up my own, but I was having difficulties getting the installer to work properly. Turns out that there is a requirement that the PDT tools be run from the C: drive of your computer. Also if you’re running them from the server OS, you will need to install the Hyper-V role in order vmcreator.ps1 to function properly. I don’t recall seeing either of those things mentioned in the TechNet article, but I may have overlooked that part.

So, linked from the vmcreator.ps1 article is a great utility, [Convert-WindowsImage.ps1](http://gallery.technet.microsoft.com/scriptcenter/Convert-WindowsImageps1-0fe23a8f) that I used to create my base OS image. The utility is super handy and has a gui or cmdline version. I wimped out initially and used the gui version, pointed it an ISO of [Windows Server 2012](http://technet.microsoft.com/en-us/evalcenter/hh670538.aspx) and after a while I had a lovely [vhdx](http://technet.microsoft.com/en-us/library/hh831446.aspx) ready for vmcreator.ps1.

After renaming the half dozen vm’s the script had created for me, in record time btw, I ran the installer.ps1. There’s not really a whole lot mentioned on the article about it’s use, it is rather self-explanatory and once you realize the limitation to the C: drive then it’s a no-brainer. That part took me a bit to figure out as I had an external drive with all the bits the [downloader.ps1](http://blogs.technet.com/b/privatecloud/archive/2013/02/09/deployment-the-pdt-downloader.aspx) had downloaded for me.

The end result is I now have the basic System Center infrastructure that I can play with locally to try out new features, or test the scripts and apps I create for work. It was really very slick, and I could totally see how I would use something like this in our QA environment at work.
