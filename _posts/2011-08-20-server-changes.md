---
layout: post
title:  "Server Changes"
date:   2011-08-20 13:12:00 -0600
categories: blog
tags: August 2011 Personal
---
So I hopped services again, and it’s taken me a bit to get things up and running the way I want them to. The big difference now, I’m hosting my own services, so there was a learning curve for me to get things working properly. This site has been up for a while, but I’ve only recently gotten my subversion site online. The site that caused me the most trouble was my trac site. Turns out my issues had nothing to do with trac at all, but TMG 2010.

The problem I had with the site was, I could get it online just fine, browse the content, and login to check the admin settings. But it wouldn’t let me log out, well it would, but wouldn’t. I would login, close the browser, dump the cache and cookies and when I opened the site back up, I was still logged in, kind of. It was very frustrating, I tried configuring the site back to default settings, same problem. I tried creating a new blank site, same problem. I even tried setting it up on a different server, thinking there was some problem with my servers configuration.

Today I had some time to troubleshoot, and started rolling through each of the rules on firewall. Now I’m no newb to Microsoft Firewalls, I’ve run every Proxy they’ve had since they called it Proxy Server. But I’ve not really had a setup with no clients behind the firewall, so the default rules for caching were just fine. Silly me, didn’t check this when I set up the TMG 2010 firewall.

I could create an exception rule to prevent caching for specific servers behind the firewall, but since I don’t really surf from my server or have clients behind the firewall I just turned it off. So for future reference while RTFM is always true, I think perhaps in this situation it was doubly true.

The short of it is, everything’s back up and running and I can get back to my other stuff. Since I’ve moved I have 5 articles queued up and now I can get back to them.
