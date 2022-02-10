---
layout: post
title:  "SCCM + Dell-ServiceTag = Headache"
date:   2011-08-06 13:13:00 -0600
categories: blog
tags: September 2011 KU SOECS
comments: false
---
Well, it’s been a long time since I’ve posted anything and this will most likely be the last post before I hop platforms, but since the resolution took nearly my whole morning I felt it was worthy a posting. As you know we’ve been rolling with System Center Configuration Manager for nearly two years now and while it doesn’t necessarily follow best practices, it’s been as stable as that product appears to be.

So today I was given a machine to image, and I promptly checked our servers to see if it was still lurking out there somewhere.

* In AD? Nope.
* In SCCM? Nope.
* In DHCP? Yes, but that’s ok.

So I fired up the SCCM console and expanded leaf objects until I got down to Computer Association. Right click and choose Import Computer Information and follow through the dialogs. What we’ve always done is add the computer by Name and MAC address, mainly because the GUID is entirely too long for any normal person to remember or have the time to write down. After adding the computer to the proper collection and finishing out of the wizard, I fired up the computer and hit F12, and let the PXE do it’s magic.

Sadly, there was no magic, after about a minute I heard two beeps which means the computer is unable to boot off the network. No worries, I know my view on time is different from that of SCCM so I waited about 5 minutes and tried again, still two beeps. I decided that perhaps I had typoed the MAC address, so decided I’d try it again; two beeps.

That’s when I began my trouble-shooting, the first thing I did was double-check the MAC, as well as make sure there were no duplicates in DHCP, which based on how we roll DHCP is impossible, but still doesn’t hurt to check. There is a report in SCCM that let’s you know if there are duplicate MAC’s inside it’s database, “MAC – Computers for a specific MAC address.” This will let you know if the MAC you enter is associated with more than one computer, it wasn’t.

I fired up the log and saw the following message, I decided to leave the typo intact, since that’s the way Microsoft left it!

> The SMS PXE Service Point intructed device to boot normally since it has no PXE advertisement assigned.
> Device MAC Address:00:1A:A0:B9:EF:A8 SMBIOS GUID:4C4C4544-0000-2010-8020-80C04F202020.

That seemed odd to me, since I knew that everything was set properly, I decided to restart the Windows Deployment Services (PXE) service. Often times that will fix small issues with PXE booting workstations for imaging, two beeps. That wasn’t it, so then I had to go to the bad place, SMSPXE.LOG. I’m not sure why, but apparently the SMS dev’s decided to punish admins and write absolutely horrible log entries that look like XML and reference line numbers in the source code. Sadly, there wasn’t much of anything different in here either except this:

> Device found in the database. MacCount=1 GuidCount=4]LOG]!>

See what I mean by horrible? Anyway, the interesting tidbit is GuidCount=4, wtf? So a while back Carson wrote a report in SCCM that would show GUIDs, I suppose I should post that at some point because it is SUPER handy! But sure enough there were 4 computer’s with the exact same GUID. All of them but one were current, so I decided to nuke them, restart PXE and attempt my boot again, two beeps. I was not a happy camper.

So it was off to Google, since I wasn’t having any luck with the logs. As you can imagine there were lots and lots and LOTS of threads, postings and technical documents.  Most of what I read was from the Microsoft Technet social site, but as I began reading I began noticing that several of these were referencing Dell Optiplex computers. While not the same vintage Optiplex as what I was reading about I was working with Dell hardware nonetheless.

I finally fell on the answer in a two year old [thread](http://en.community.dell.com/support-forums/servers/f/177/p/19265657/19454461.aspx#19454461) on the Dell support site. Turns out that service tag is more than just helpful on their website! The GUID for the computer is based on that service tag, and if the motherboard gets replaced and the tech doesn’t add it back into the BIOS, the computer will create a generic one. That’s the GUID or SMSBIOS GUID listed above, and it was a painfully easy fix! You will need to download the [ASSET.COM](ftp://ftp.dell.com/utility/ASSET_A209.COM) utility from [Dell’s Utility FTP site](ftp://ftp.dell.com/utility).

Once downloaded run that command with the /S switch and the service tag for your computer. Please be aware that if you muck up that entry, there is no way to remove it! So double, triple, quadruple check before you confirm that it’s ok to update that information.

The only other complicated part for me was finding a floppy drive, and more importantly a floppy disk to make bootable so I could run this and a BIOS update on the intended computer!
