---
layout: post
title:  "Week In Review: 05/25/2014"
date:   2014-05-25 12:01:00 -0600
categories: blog
tags: May 2014 KU IT Scripting Review
---
Well it’s been forever since I’ve written anything interesting so now is as good a time as any. Recently we were informed we needed to start keeping track of time spent on projects and since this is something I did for a couple of years at the School of Engineering, it’s not too difficult for me to get back into the swing. Although this go around I went with more of a journal style, not sure I like it but we’ll see.

LOTS of programming this week. I wrote a virtual machine provisioning app a while ago, and while functional I’m not sure how many folks actually use it. But there has been some renewed interest lately, mainly around removing a lot of the paperwork involved. So I’ve gotten to get my hands dirty playing around with various web services.

While not terribly fleshed out right now what I’ve got works for what we need.

* I can communicate with VMware to provision a server
* I can send the vlan information over to Proteus (Bluecat) to ask for the next available IP in the network
* I can submit the details of the server, ram, cpu, disk, network information over to ServiceNOW for inventory
* I can automatically generate tickets for handling backups and Zenoss monitoring
* I can also talk direct to Zenoss to get the server into the system

I find more and more that programming is becoming more important for administering servers than perhaps it once was, or I’m just going off the deep-end with programming 😉

Here are the projects on GitHub associated with i’m working on now

* [mod-vmware](https://github.com/jeffpatton1971/mod-vmware)
* [mod-servicenow](https://github.com/jeffpatton1971/mod-ServiceNOW)
* [mod-zenoss](https://github.com/jeffpatton1971/mod-Zenoss)
* [mod-proteus](https://github.com/jeffpatton1971/mod-Proteus)
* [mod-hyperv](https://github.com/jeffpatton1971/mod-hyperv)

You will note that I have a hyper-v module, but I’ve not talked about doing hyper-v at work. We actually have a little test cluster that we spun up earlier this month to start kicking the tires.

I’ve also been talking a lot with Microsoft. We’re working through an issue where provisioning users for Lync sometimes fails. It’s incredibly intermittent and next to impossible to reproduce. I’ve taken to having the guys turning on PowerShell logging (start-transcript) before they do anything just in case they catch it so we can pass that on to Microsoft.

Spent a few hours talking with one of their SQL support guys and now when an error occurs during provisioning, in addition to sending the error out to file; I also run a query that grabs data from one of the system tables regarding communication.

What else…System Center Advisor preview is AWESOME! I’ve been talking with a program manager at Microsoft as well as one or two guys who develop it about some feedback I had given and some issues I was having. Gotta say that’s been super fun, would so love to work there!

Oh! Finally got the monitoring VLAN all setup and started moving my servers into it. Had some fun issues there, first I couldn’t get to DNS so no name resolution, no accessing servers by names…fun times! Then, I forgot to file the change paperwork for changing the IP addresses of the servers, so the firewall rules never got updated…sigh

I’ve spent a fair amount of time getting Operations Manager all happy and cleaning up the various Management Pack issues that I’ve not dealt with since I’ve not been able to communicate with the servers. One of the more challenging parts for me lately has been getting the Low Privilege SQL monitoring working, I think I’ve got it all worked out now though so we’ll see how that goes next week.

In addition to being able to access the servers from the monitoring VLAN it also appears we have just about the same level of access from our desktops! No more RDP’ing into a dozen servers to do something like tweak a registry setting or stop a service!

Oh well, that’s it for this past week. I hope to start doing some more writing but I’ve decided to at least do these Week In Review posts.
