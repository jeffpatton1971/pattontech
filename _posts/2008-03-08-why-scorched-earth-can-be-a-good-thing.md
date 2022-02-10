---
layout: post
title:  "Why 'Scorched Earth' can be a good thing"
date:   2008-03-08 11:01:00 -0600
categories: blog
tags: March 2008 KU SOECS
comments: false
---
Historically here in the School of Engineering, software installation was handled by a single person. What I mean by this is, if a faculty person required specific software to be installed, one individual would take the time to get that software to work. Little if any documentation was created as we relied on this one person to always be there.

Currently, our image is very poor. We lack the documentation, (documentation = knowledge), to properly configure the upwards of 60 unique applications needed by the various disciplines within the school. Consequently, when an application is found to be missing, or mis-configured it takes a significant amount of time to re-invent the wheel. Because of that confidence in our ability has suffered extensively.

So the only real solution is to start over, and in our environment the only real way for us to do this is to blow everything away. Come up with a new way of delivering the requisite service that faculty, staff, and most importantly the students require. To that end I have a very simple plan that I will begin to implement.

Step 1.

Create a new way to deploy software.

All software will be packaged into an easily deployed .msi file. Each .msi file will be deployed via GPO attached to the appropriate OU in the Active Directory.

Step 2.

On the first full week after classes end; systematically wipe and re-install each individual lab, using Windows Deployment Services combined with Managed Software Installation and Windows Software Update Services.

The end result should be a lab hat is fully configured and updated with software that works. I was intentionally vague on the “new way” process as I will detail that in a second posting, but this is basically the idea.
