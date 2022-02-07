---
layout: post
title:  "Managing a Server Core fileserver"
date:   2010-12-29 19:38:00 -0600
categories: blog
tags: December 2010 KU SOECS
---
So, you want to manage that Server Core now I suppose. Well, ok but you will need to do a few things first…

As we all know, there isn’t much of an interface on a Core server, just a command shell. For some that is a bit intimidating, for others it’s a welcome sight. But at some point you will want to do something from a different computer, maybe use one of those nice GUI tools like Server Manager, Share and Storage Manager, File Server Resource Manager and any other kind of manager you can think of. It’s ok, I wanted to use them too!

Management Workstation

You will need to do a few things first, load the proper tools and set some firewall settings. For the purpose of this article I’m assuming you are running a version of Windows 7 greater than or equal to Windows 7 Professional. You will need to download and install the [Remote Server Administration Tools for Windows 7](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=7d2f6ad7-656b-4313-a005-4e344e43997d&displaylang=en), for a detailed list of what is available view [KB958830](http://support.microsoft.com/kb/958830/en-us). Once you have the set of tools installed that you want to use you will need to open up some ports in the firewall. The following may or may not need to be enabled depending on what you’re wanting to do.

* Remote Administration
* Remote Event Log Management
* Remote Schedule Tasks Management
* Remote Service Management
* Remote Volume Management

Windows Server 2008 Core

I’m not going to explain all the different roles and such that are available, but I am toying with the idea of writing up a guide or something that will lead you through some basic server setups. For now, I will assume you are just needing to manage your file server, and that you have already installed the necessary roles and features to do so. If not, check out my last [article]({% post_url 2010-12-28-windows-server-core-2008r2-file-server %}) to see how to do that. You will need to have PowerShell enabled prior to setting up the Server Manager stuff, but there is a nifty little command available to help us out, sconfig.

The text below has been copied from [KB976839](http://support.microsoft.com/kb/976839).

For a remote server that is running a Server Core installation of Windows Server 2008 R2

Method 1

1. Log on as an administrator to the remote server by using the console or by using Remote Desktop Services.
2. Open an elevated Command Prompt window.
3. At the command prompt, type the following command, and then press ENTER: sconfig
4. At the command prompt, type 4 to select the Configure Remote Management option, and then press ENTER.
5. At the command prompt, type 3 to select the Allow Server Manager Remote Management option, and then press ENTER. Note If you have not already installed PowerShell, you are prompted to do this. Type 2 to select the option to install PowerShell. Click OK when you are prompted that the installation of PowerShell is finished. Additionally, you must restart the server before you complete step 5.
6. Connect to the remote server by using Server Manager. Note You do not have to restart the remote server unless you have to install PowerShell before you complete step 5.
