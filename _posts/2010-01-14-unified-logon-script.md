---
layout: post
title:  "Unified Logon Script"
date:   2010-01-14 19:00:00 -0600
categories: blog
tags: January 2010 KU SOECS Scripting
comments: false
---
This script is entirely too long to publish as a page, you can find it in my public repo at the following URL.

[Unified Logon Script](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/UnifiedLogonScript.vbs)

This script is the culmination of roughly two years of tweaking. It started out in May 2008 and was tweaked throughout that summer, it’s final update was at the beginning of December 2009. The script above was created in August of 2009  and we never actually pushed it out to the labs. Sadly this script will never make it into production due to our current move to Windows 7, many of things that were once easy are now complex.

But let me tell you about the really cool features of that script, firstly if you cruise through my VBS book you will find many of the functions that I use in random scripts are present in this one. The script itself is fairly well documented as to what it is doing. The best part of it is how it works the Active Directory to get done the things that need done.

We manage several labs between two buildings, these labs make up about 50% of the computers we maintain. Lab students are provided with a consistent user experience across our labs regardless of where they logon from. The large portion of this is done through redirected profiles, which could be an article all on it’s own! The other part of the equation are drive and printer mappings. All lab computers receive the same set of drives:

* U: user drive and personal information is stored here
* L: a drive used by some classes for storing data to be shared
* R: a project drive for the various project groups within the school
* P: a publicly available read-only drive that stores backgrounds, bug fixes and some documentation

These drives need to be available on all computers, historically this had been managed through batch files. While useful you are limited to what you could do with a batch file. So we started using AD and scripting to do some of the work for us. We created 4 shared folders at the root of the Labs OU, the name of each object was the drive letter, and the path was the path to the share on the server. In a similar fashion we did the same thing with printers, we created those as shared folders as well, this allowed us to have one printer in multiple OU’s. Each lab has an OU and one or more printers are then created in that OU.

This is where the cool part of the script kicks in! A user logs into any lab computer, the script asks the computer what OU it’s in. It then searches that OU for any object with a UNCname property and then passes that object off to a procedure that will map it. We determine what it based on what server the object points to. Once it’s done in the home OU, it then works it’s way up to the OU’s in the tree above until it reaches the root of our AD. Along the way it maps any object it runs across, if your user account is a member of a group that had read access on the object you can map it, otherwise it doesn’t map.

With only a few minor changes this script can be placed in almost any AD and work through mapping printers and drives. Sadly, with the release of Windows Vista and UAC much of what the script can do has been mitigated.
