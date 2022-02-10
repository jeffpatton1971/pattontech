---
layout: post
title:  "How we use the Active Directory"
date:   2008-10-20 11:22:00 -0600
categories: blog
tags: October 2008 KU SOECS
comments: false
---
We rely on a lot of Microsoft tech to keep things clicking here at the School, and nothing is more important than our Active Directory Infrastructure. When I first started about the only thing you could get out of the Active Directory was a list of computers, users and groups.

Currently, we use the Active Directory as much as possible. One of the scripts that I posted a while back performs our inventory and actually stores pertinent information regarding each computer in it’s description property. One of the cool things that I’ve started doing, and I know I may behind the curve here, is to place objects in the Active Directory.

Two things that are incredibly important to users are accessing their data, and printing it out. So I have taken advantage of our OU structure, based on location, to make this much simpler. I have moved all the PrintQueue objects out of the print server and placed them in the office OU where the printer is located. In addition I have started publishing all of our shares into the Active Directory, more at the building level as most drives are fairly common for everyone, but there are some that are in individual offices.

Then I modified my scripts to take advantage of this, basically at logon the scripts figures out what OU the computer is in, and starts mapping PrintQueue or SharedFolder objects there, and then progressively works up the tree until it gets to the root of the Active Directory. This has made things much simpler to manage as I can literally just edit the object if a server changes and the script automatically maps to the new location or server!

The scripts will be posted as soon as they have been commented.
