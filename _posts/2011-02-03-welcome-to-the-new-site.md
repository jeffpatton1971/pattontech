---
layout: post
title:  "Welcome to the new site"
date:   2011-02-03 13:01:00 -0600
categories: blog
tags: February 2011 Personal
---
I’ve been running on an OpenVZ server from [PhotonVPS](http://photonvps.com/) for quite some time now. That server was running on Ubuntu Server 9.10 (I was wrong Carson), and the site was running on [Django](http://djangoproject.com/). I’ve had no complaints for the most part, Photon has always been very prompt and aside from the few minor annoyances which seemed to plague Carson and Nick more than I, it’s been fun.

Over the past weekend I decided to look for a different hosting provider, and was wanting to change the sites look and feel. Since I am at heart a Windows guy I felt I should move over to a Windows server. I looked at the [Photon Hyper-V service](http://photonvps.com/hyper.html), and compared that to the [AccuWebHosting Hyper-V service](http://www.accuwebhosting.com/windows-vps-server-hosting.html) and decided that for the money, AccuWebHosting was where I wanted to be.

The past few days we’ve had some crazy cold weather and so on Monday I moved my site over to the new server. I was able to setup [Apache](http://httpd.apache.org/), [Python](http://python.org/) and [mod_wsgi](http://code.google.com/p/modwsgi/) and get my new server up and running in about 20 minutes, thanks to an [article](2010-07-01-django-and-windows.md) I wrote a while back. Then I started poking around at an alternative to Django on IIS (which I’ve not got working…yet). I found [BlogEngine.NET](http://blogengine.codeplex.com/), and so far I really enjoy it, it feels very [Drupal-ish](http://drupal.org/) and I don’t think that I’ll stay with it for the long haul, but it works for now.

Over the next few days I’ll make more tweaks and move the rest of my content over.
