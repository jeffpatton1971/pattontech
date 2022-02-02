---
layout: post
title:  "Test Deployment"
date:   2007-06-29 08:56:09 -0600
categories: KU SOECS Lab
tags: June 2007
---
Using Remote Installation Services I deployed Windows XP SP2 onto my test machine, Dell Precision 390. Once the image was in place I installed the base load that all Lab computer must have available. This install comprises approximately 30 applications ranging from something as simple as [RoboCopy.exe](http://www.microsoft.com/downloads/details.aspx?FamilyID=9d467a69-57ff-4ae7-96ee-b18c4790cffd&DisplayLang=en), to applications as complex as [AutoCAD 2007](http://usa.autodesk.com/adsk/servlet/home?siteID=123112&id=129446).

After completing the installation of all the software, I proceeded to clean the computer up, Empty Recycle Bin, Clear Document List, Clear Application List and Degragment the hard drive. I connected to the Remote Installation Share and ran the [RIPrep.exe](http://technet2.microsoft.com/WindowsServer/en/library/a6e23f3c-c982-49b4-940d-bd0fd54cd4201033.mspx?mfr=true), and with the exception of one error everything ran smoothly. I will need to reinstall the [Eclipse](http://www.eclipse.org/) software through a GPO.

I sucessfully connected two test machines, both Dell Precision 390’s and the imaging appears to be going smoothly, will have to test the images on Monday.
Thoughts:

Putting this in to production there will be a few things to consider, first KU uses [ANSR](http://sourceforge.net/projects/ansr) for all their DHCP needs. I need to find out if ANSR supports the [PXE Extensions](http://www.3com.com/other/pdfs/infra/corpinfo/en_US/pxe.pdf) required for RIS to work properly. KU also uses BIND to handle all name registrations and I need to see if they comply with [RFC 2052](http://www.ietf.org/rfc/rfc2025.txt), [RFC 2136](http://www.ietf.org/rfc/rfc2136.txt). Might possibly have to use a private network to deploy image to lab computers, perhaps a “mobile deployment station”.
