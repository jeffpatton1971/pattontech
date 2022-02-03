---
layout: post
title:  "The usefulness of TCPDUMP"
date:   2010-09-08 19:23:00 -0600
categories: blog
tags: September 2010 KU SOECS
---
We are mid-firewall migration, it’s set to happen in about 2hrs from this post. But in the middle of trying to figure out why we are getting crazy network outages, we ran tcpdump on our dns/dhcp server. Why that server? That was the server that was identified by Central-IT as having issues.

We only ran tcpdump for a few minutes and in that time we got some very interesting results. It appears that another entity on campus is hammering our dns server with known bad dns zones. I filtered on just that subnet, selected those packets and exported it to a csv. We have a plotter, so of course the next thing to do was plot it out! Sadly, Excel decided to add page-breaks…sigh

There were 23,000 packets captured and this entity accounted for 4,400 packets. I’m no math-wizard, but that seems to be about 5% of the traffic, in a very short time frame.

But the pic is awesome! Right?

![20100908-Photo1308](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100908-Photo1308.jpg)
