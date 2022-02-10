---
layout: post
title:  "Microsoft Clustering"
date:   2008-10-13 11:19:00 -0600
categories: blog
tags: October 2008 KU SOECS
comments: false
---
Some things I’ve recently learned while working in a non dynamic environment.

* Clusters by nature are dynamic
* Even when you don’t think it matters, it so does!
* If DNS is broken, nothing else matters.

In a non dynamic environment, set your Resource Name as a static entry in DNS. With a static DNS entry it should be safe to then uncheck the box for “DNS registration must succeed.” The idea being that if you have the static entry pointing at the correct IP, then DNS just works.

In this type of environment it appears that if that option is checked, which I believe is the default, and your cluster fails-over, those resources may not come back online. Given our current painful implementation of DHCP/DNS here, a simple configuration change took close to two hours to effectively work.
