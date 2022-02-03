---
layout: post
title:  "One admin, multiple domains"
date:   2010-09-03 19:22:00 -0600
categories: blog
tags: September 2010 Personal
---
So I want to be able to have a single Django admin site for more than one domain. After talking it over with [Carson](http://carsongee.com/), he showed me what needed to be done. It’s really quite simple when you talk to someone knowledgeable, and go over it from beginning to end.

Django Setup

First, setup your sites in whatever way makes sense to you. I have created a folder at the root called /sites, within that folder I created a project for each of the sites I want to host, and named them for their registered domain names. Second, each site that you want to have show up under the main admin site must have a unique SITE_ID. Third, those sites must all be attached to the same database on the backend.

The last important bit is this, only enable the admin site in the urlconf of the site that should have it. Then just add your sites to the sites table in the admin interface, and assuming your apps are site-aware, you should be good to go.

Apache Vhost Setup

Now that you have all the Django stuff out of the way you just need to setup Apache, each domain with it’s own vhost. Most likely that’s how you already have it setup, so there may be very little if anything to do.

Problems I hit

Site matching query does not exist: Turns out I added my two sites, and deleted the example. This screwed up the SITE_ID in the database. So example.com is SITE_ID 1, and your subsequent sites are sequentially numbered after that. If you mess it up liked I did you will need to manually adjust those entries in the db. Use whatever method your backend db provides for updating those records.
