---
layout: post
title:  "Unified Startup Script"
date:   2010-01-14 19:01:00 -0600
categories: blog
tags: January 2010 KU SOECS Scripting
---
Following along with the Unified Logon Script, there is also a Unified Startup Script. Again with the release of Windows 7 much of what we did via script technologies is now handled differently but I wanted to document what the purpose of this script was as well. It is also quite long so you can find it in my public repo at the following URL:

[Unified Startup Script](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/UnifiedStartupScript.vbs)

While we had a single logon script to handled the balance of our user experience we had a couple of places to handle the things that make all of our software go. Around the same time as I created the Unified Logon Script I also began working on a Unified Startup Script. This script runs at computer startup and therefore runs in the SYSTEM context of the computer. This script is responsible for defining all of the environment variables for licensing that allow the over 100 engineering applications to work. Additionally it sets up two folders on the computer that are required for some software to work, it also clears those folders. To add to that we also can define who is to be local users on the workstation as well as defining the administrator account password which is passed in as a parameter from the GPO.

As this script runs only at startup we force a reboot of lab machines once a week.

It is similar in structure to the Unified Logon Script, but lacks the AD portion as it’s not necessary to the functionality of the script.
