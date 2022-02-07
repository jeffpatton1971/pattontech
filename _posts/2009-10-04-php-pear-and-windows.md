---
layout: post
title:  "PHP, Pear and Windows"
date:   2009-10-04 14:15:00 -0600
categories: blog
tags: October 2009 KU SOECS
---
When running PHP I receive the following message:

> “This application has failed to start because MSVCR71.dll was not found. Re-installing the application may fix this problem.”

This file is available on your system, it is part of the .net 1.1 installation and you can find it in the appropriate folder. What I’m guessing at is that when PHP was compiled this file was linked directly in and on the machine doing the compiling it was located in the system32 directory. I tried several things to make it go.

1. Download and attempt installation of .net 1.1: FAILED
2. Attempting to use REGSVR32.exe to register the file: FAILED

What appeared to make the error go away was much simpler, I copied the file from it’s location in the .net 1.1 folder structure to the system32 directory and I’ve not seen the error. Below is the command line that I used to make it work.

``` powershell
copy C:\WINDOWSMicrosoft.NETFrameworkv1.1.4322MSVCR7.dll C:\WindowsSystem32
```

I do know that the lack of the file does not appear to cause any problems with PHP at all. PHP pages load perfectly fine, running PHP scripts from the command line also work fine.

The Web PI installation of PHP lacks several extensions that may be necessary based on your needs, so in order to get the needed packages you will need to use the Pear package manager. In addition to lacking many extensions, the Web PI install of PHP also lacks Pear, fortunately this is very easy to get.

I opened the Pear [manual](http://pear.php.net/manual/) and in the section under installation found that all you need to do is download the [go-pear.php](http://pear.php.net/go-pear) file and run it. Follow the prompts and answer the questions, on my base installation it detected everything properly and was able to install and configure itself with no problems.
