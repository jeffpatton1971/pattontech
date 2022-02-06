---
layout: post
title:  "Week In Review: 07/27/2014"
date:   2014-08-02 12:07:00 -0600
categories: blog
tags: August 2014 KU IT Scripting Review
---
Another week coding the app, the nice thing is that the MVC re-write is pretty much done. In fact the idea of utilizing the DB to store information is actually in place, something I wound up having to do. I have decided to store a snapshot of the relevant data in some tables at login. The actual collection of data and table insert takes roughly 20 seconds, so it’s pretty quick. This speeds up everything quite a bit, the entire site feels a lot more responsive.

I’m also having to cron an insert for the proteus data every 24 hours. There isn’t a simple way of to query the underlying properties of a given ip4Network, so I’m just going to collect them all. There are roughly 4,000 ip4Networks defined and it takes about 2 minutes to enum each one and write the underlying properties to a table. Since that data is much more static than the VMWare data, 24 hours on insert is sufficient.

I’m currently making much better use of my classes, and passing data back and forth between views with them. On two of them I have over-ridden the ToString() method so that I can just class.ToString() to get my confirmation email, it looks really slick in code. The added benefit of what how I’m doing this now is that I only store the friendly names in the class, so displaying the information back out is super simple. Then just a couple of simple queries to return the information I need.

I have tweaked a few things on some of the projects that are used.

* I have written a slightly better way of getting to the cloned network adapter than what I was doing in the past.
* A lot of my custom validation code has slimmed down, since for these I return a bool, I don’t need to define an object to hold the result and check that, I simply check the call.

As the rest of the code is already done, I figure I have a couple more days then we can call it done. The only major thing left for me is to build the method to create a new virtual machine, and the little I’ve looked into it, it appears it’s nearly identical to how you clone a vm.
