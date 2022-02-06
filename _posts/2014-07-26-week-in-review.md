---
layout: post
title:  "Week In Review: 07/2/2014"
date:   2014-07-26 12:06:00 -0600
categories: blog
tags: July 2014 KU IT Scripting Review
---
I had intended these to be weekly, and I got off track, sorry about that. So I missed the week of Jun 29, Jul 6 and Jul 13. I can tell you that the week of the 29th ended on July 5th, and I was pretty wiped out from all the fireworks and stuff. The week of the 6th we were in Florida on vacation and the last couple of weeks (Jul 13 and this week Jul 20) were about the same.

This week has been all about the code. My boss asked me to focus on getting the revised provisioning web app up and running, so I’m happy to report that it is on par with the previous version. The previous version was just a basic web app, nothing fancy, it had some pages that worked like a form and it was all driven by click events and such. It worked really well, but it lacked flexibility.

See, everything was written on a single page, the various “pages” were in reality Asp Panels separated by div’s. In order to add a feature I would have to figure out where to place it, then adjust all the code to handle it. Some of that was problem, I should have broken things out more earlier into separate functions, that may have made it easier, but I doubt it.

The new version is using MVC, each page has a model on the back to represent the data. So each page is strongly typed, For most things I use the built-in validation provided to me from that, for a few things I wrote some custom validation, for example to make sure someone chooses a valid option from a drop down and not the text “–Select an item–“. There are a few places where the custom validation just wasn’t appropriate. So I was able to create a couple of functions to handle that, I must say that in this new format it is a lot easier to work.

The way I have it now I just pass around the models from one page to the next, updating as we click along. I can pass those models to my custom validation and return a simple error message quite easily. Displaying the validation error’s previously was a pain, so I really enjoy the way this works.

Another nice feature is I can re-use a lot of what I do so I can provide new features. For example, I want to provide a way for admins to create a bare VM. I can use all the models I already have, as well as just about all the logic behind how that is wired up. If I want to add a new feature it’s as simple as writing up a model and then building a page for it, then writing a controller to handle it all, or updating an existing controller.

Some thoughts I’m having for the next version is to use the database, currently I’m not doing that at all, I’m actually querying VMware directly, this introduces some lag that the previous version didn’t have. I think that some of that can be resolved by me working through my code to get it optimized, but a lot of that is just inherent in how it all works.

So the thought was a separate job that would at regular intervals poll VMware for clusters, datastores, vm’s and so forth. Then the app becomes almost instantaneous, since instead of asking VMware we’ll ask the database. Then before we provision we will validate everything against VMware, which may introduce some entertaining issues, but as our change process is fairly slow I think this will work nicely.

The nice thing about storing this data I can add tables to give me logic that I had to code for before. I will be able to access some data without calling VMware since it will be stored in the database and some of the code I wrote to pull data from VMware I can dump as it won’t be needed.

So what all code am I using?

* mod-VMware : allows users to connect to, query and clone (soon create) virtual machines
* mod-proteus : allows users to connect to, query and add host records to Proteus and retrieve IP information for new hosts
* mod-servicenow : allows users to connect to, query and create service tickets and configuration items
* mod-Zenoss : allows users to connect to, query and add devices to Zenoss monitoring
* mod-ads : allows users to connect to, query and create objects in Active Directory

I feel that I’m getting closer to my idea of being modular, where we can plug in the various things we want. I still haven’t worked that out yet, but in my head it works 😉

enjoy!
