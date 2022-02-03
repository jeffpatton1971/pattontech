---
layout: post
title:  "Fun things to know and share about the Dell PowerEdge NX3000 NAS Appliance"
date:   2010-11-15 19:33:00 -0600
categories: blog
tags: November 2010 KU SOECS
---
This was originally going to be a post about setting up a cluster on the NX3000, but I’ve had some fun messing around with them, so I thought I would share that and make a second post for the setup. Both Carson and Nick would likely attest to the fact that I’m a little picky about how my servers are setup (although I’m not nearly as Spartan as Nick can be). I have never left the factory setup on the servers that I’ve put out, and personally can’t imagine anyone doing that. So anyway, these arrived about a month ago and unfortunately my time has been consumed by printer issues and I’ve not had a chance to work on them.

![20101119-10192010002_2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101119-10192010002_2.jpg)

For whatever reason Dell will cut the physical drive in half, a System partition (C:) and an empty Data partition (D:), this is one of the things I find personally annoying and usually change it. The only problem I ran into was using either the OS DVD or the System Builder DVD I was unable to change the default partitioning that Dell uses. I will on occasion use the System Builder as it allows me to pump in all the information that setup asks for later, but apparently if you are going to mess around with an NX3000 NAS Appliance, you shouldn’t use the System Builder DVD.

But before I get to that, lets look at some fun pictures of the System Builder DVD. In this first one you will notice that I only get one choice of OS to select from. The thing I want you to notice is the checklist off to the right, you will notice that there are several steps in this process. But what happens when we click continue on this screen?

![20101119-WP_000024_2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101119-WP_000024_2.jpg)

Well what should jump out right away is that I seem to have lost a few steps in my setup, in fact we went from 7 steps to 4, and instead of being at the beginning of the process we’re almost done! What happened to everything in between? Nobody will give me a good answer, but one answer I got from a Dell tech was, “it’s easier.” Ya, I don’t know what that mean’s either but it’s as good an answer as any!

![20101119-WP_000025_2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101119-WP_000024_2.jpg)

So if you can’t or shouldn’t use the System Builder DVD what should you do? According to a different Dell tech, with this appliance you can only use the OS DVD, when asked why they packed the System Builder in there, I was told it has useful tools on it…sure it does.  Well the fun part is after I went through this, everything seemed happy and wonderful. The server rebooted, I saw the Windows 2008 starting up screen, and then I got the following message.

![20101119-WP_000070_4](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101119-WP_000070_4.jpg)

The System Builder only had one OS choice selection, “Microsoft Windows Server 2008 x64 SP2”. My OS DVD read, “Microsoft Windows Storage Server 2008 Enterprise With Service Pack 2”. The error message reads, “Valid Microsoft Windows Server 2008 media was not found”.

![20101119-WP_000068_4](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101119-WP_000068_4.jpg)

Anyway, here are some entertaining screen shots from using the OS DVD.

![20101119-WP_000020_2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101119-WP_000020_2.jpg)

Ok, so I took these from my phone so they are a bit shaky but you get the idea, I really like the Display Log Console on the right. Nothing EVER showed up in there, EVER.

![20101119-WP_000021_2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101119-WP_000021_2.jpg)

I know I’m being nit-picky, but really? My OS is Landing? Wouldn’t “loading” make more sense? In my last [post](2010-11-15-redeeming-my-it-card.md) I mentioned that Carson gave me a hard time for not looking at the default web site.

![20101119-WP_000071_2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101119-WP_000071_2.jpg)

There was absolutely nothing there, if I had installed the Server Manager I would have seen something, but nothing cool by default.
