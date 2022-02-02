---
layout: post
title:  "Windows/Linux Server Base Image"
date:   2009-10-02 14:12:00 -0600
categories: blog
tags: October 2009 KU SOECS
---
I do a lot of software evaluations and as such I have come to rely quite heavily on virtualzation so I don’t have to worry about setting up hardware. As the software that we evaluate can be either Windows or Linux based I have created two base images, and from there build out my test servers for software deployment. The steps for each OS are very similar and I won’t go into any great detail on specific setup steps. I tend to just do a basic Windows Server installation taking all the defaults, and likewise for my Ubuntu LTS installation with only exception being selecting the OpenSSH at the end.

My personal choice for virtualization software is Virtual PC, mainly because I have used it ever since it became available for Windows and I don’t need anything more complicated to evaluate software.

Virtual PC Settings:

* Networking: Shared NAT
* Memory: 512MB
* Hard Disk 1: 40GB
* Floppy: Disabled
* Sound: Disabled
* Hardware Virtualization: Enabled

I have .iso images of my server install disks, so I just boot the blank base vm from iso and run setup from there.

Windows Server Installation:

* Default WIndows Server installation
* Apply the most recent service pack
* Install .net 2 & 3
* Connect to Windows Update (critical and software)
* Repeat until all updates applied
* Copy the i386 folder from the cd to drive C:
* Extract supptools.cab to C:Support
* Run the Setup Manager and choose a sysprep install
* Sysprep the server and shut down

Ubuntu LTS Installation:

* F6 and add noreplace-paravirt
* Default installation
* Choose OpenSSH at the end
* SSH into server modify menu.lst add vga=771 noreplace-paravirt
* sudo apt-get update
* sudo apt-get upgrade
* sudo shutdown -h now

I’m not sure if there is something similar for sysprep in Linux, but I’m pretty sure that it doesn’t really matter as you can just edit files in /etc and you’re done.

That’s pretty much all I do, the fun stuff is when I create a new machine based off this one.
