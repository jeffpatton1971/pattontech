---
layout: post
title:  "Deployment Results"
date:   2007-07-02 09:01:00 -0600
categories: blog
tags: July 2007 KU SOECS Lab
comments: false
---
Results:

* Test computers both appear functional
* All Applications appear installed
* No errors reported in the System Log
Thoughts:

In my experience when a Windows DHCP Server is online clients will retrieve IP’s from it instead of ANSR. It was explained to me that ANSR takes a little
bit longer than the Windows DHCP Service, and since DHCP is “first-come, first-served” clients will almost always get their IP from the Windows DHCP
Service.

* Create a private network on the KU LAN
* Assign private IP addresses to DNS, DHCP, an AD Servers
* Create a private scope on DHCP
* Create reservations for each lab computer, to prevent regular client from getting a non-routable IP
