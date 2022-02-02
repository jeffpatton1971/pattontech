---
layout: post
title:  "Configuring Virtual Lab Environment"
date:   2007-06-28 08:56:05 -0600
categories: KU SOECS Lab
tags: June 2007
---
Virtual Lab

Using existing Virtual Domain, added two additional VM’s using a custom script I built, one was a Windows Server 2003 VM that would become RIS and the other was a Windows XP VM that would be overwritten with the RIS image.

   1. Added DHCP Service to Domain and Authorized same
   2. Joined RIS server to Domain
   3. Installed RIS and configured first RIS image (Windows XP)
   4. Added second RIS image (Windows Server 2003)
         1. To be used to image the live servers
   5. [Authorized RIS Server, rissetup.exe -check](http://technet2.microsoft.com/windowsserver/en/library/e4e14c1e-2f5e-4b8d-8dbd-7bcce1eecb011033.mspx?mfr=true)
   6. Rebooted RIS Server

The only issue I had was when I attempted to push the Windows XP Image out to the Windows XP VM, the media I chose to create the image was an upgrade, so the XP push failed, but I the client did connect to the RIS Server and pull down the GUI.

That was proof of concept enough for me, I decided to push the Windows Server 2003 Image down to the two Live Servers. We have a number of Dell OptiPlex 745’s in storage so I brought two to my office to be used as my Live Servers. The Live Servers are going to be a Domain Controller, and a RIS Server.

The first problem I came across was the OptiPlex 745 uses a Broadcom 57xx Network Card and the Windows Server 2003 Image didn’t have a suitable driver. So I needed to prepare the Windows Server 2003 Image with the appropriate drivers.

    * [Get current driver for Dell OptiPlex 745](http://support.dell.com/support/topics/global.aspx/support/downloads/en/downloads_splash?c=us&l=en&s=gen&~mode=popup&file=175637)
    * [Update the RIS Image to use the new driver](http://technet2.microsoft.com/windowsserver/en/library/4e41cc50-08ec-4f56-b75a-8519da43aeed1033.mspx?mfr=true)

While the drivers were now in the correct places I was still getting the message that there were no drivers suitable in the Image. So a search on the Dell Forums yielded a series of [posts](http://www.dellcommunity.com/supportforums/board/message?board.id=dim_other&message.id=295420&query.id=65493) that seemed to fit my problem. I followed the bold instructions halfway down the page to resolve the driver issue.

After that things seemed to move smoothly until right before the setup started I received a very ominous error message:

    File b57w2k.sys caused an unexpected error (21) at line 3903 in d:srv03rtmbasebootsetupsetup.c

I did a search on the first part of that error message that directed me to a blog [post](http://www.markwilson.co.uk/blog/2005/01/problems-with-certain-nics-and-ris.htm) about installing Windows XP via RIS. Placing the Windows 2000 driver in the i386 folder on the RIS server did the trick, the installation went through without a hitch. The last problem I received was after the installation the computer didn’t list a network card, the drivers were apparently never used.

This got me to thinking about the lab computers we have currently, Dell Precision 390’s, I have tried unsuccessfully in the past to install our Windows XP VLK but get stuck when it attempts to detect the hard drive. The problem is the 390’s have a SATA drive, I need to check if there is a Windows 2000 driver for that, which may resolve the issue I was having earlier.