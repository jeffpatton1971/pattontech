---
layout: post
title:  "Week In Review: 06/22/2014"
date:   2014-06-28 12:05:00 -0600
categories: blog
tags: June 2014 KU IT Scripting Review
comments: false
---
Not a terribly eventful week, I’ve been working on tuning Ops down and clearing out the errors, and lowering the signal to noise ratio. One of the hardest ones has been the domain trust monitor in the AD MP. So we use the firewall to isolate VLAN’s from one another and I don’t know that I totally agree with that, but one thing at a time I suppose. Our DC’s couldn’t resolve another domain to which we have a trust established, you read that right. So, I tried several different requests to make that go. I created their zone as a secondary in my DNS, had to allow TCP/UDP 53 through for that to work, still no go. Then I noticed that LDAP was being dropped on one of the firewalls so had to allow that through, still no go. Finally took out the big hammer and had those DC’s added to the same firewall group as mine. That worked, sort of, yesterday I noticed I’m still getting some errors, so I’ve decided that Monday I’m going to work on trying to get that and as much of everything else sorted out as I can.

I did get some code written this week, I have a proof of concept wired up that will allow us to copy profile information from one system over to SharePoint Online. I would feel better about it if we weren’t forced to use a deprecated web service. Which I need to bring up in our next meeting about this. The other code I worked on was the [Arin Whois-RWS](https://www.arin.net/resources/whoisrws/index.html) interface. I didn’t realize this, but Arin has a REST interface that allows you to query their Whois database.

So I’ve been working on that and wrote up a very nice little piece of code that is suitably generic for me, which I like!

So, if I want to start using the .net HTTP stuff on the Windows Phone and tablets, it looks like I have start using the async stuff. Also, I think a lot of their HTTP request stuff is moving to that anyway, so I’ll really need to keep that in mind as I progress in code writing. At any rate, I really like this GetResource function. A lot of the code I write I try to make as reusable as possible, I hate having to write code that more or less does something another piece of code already does. So we send in a string URL and cast it against the XML classes we created to return objects that we can easily work with. The disappointing part of this for me was I should be able to pass as part of the request that I want a JSON response, but the server seemed to ignore my ContentType request and kept spitting back XML.

As I was working through this I started thinking about the other code that I’ve written for VMware, Hyper-V, ServiceNow and so on. I think I’m going to re-write my provisioning web app so that I can be a little more pluggable than I currently am. For the VMware stuff I think this will be easier as I’ve already re-written that code to use the ManagedObjectReference for everything, and since at it’s root, that’s just a string this may be pretty simple. I’ll just need to create a couple of simple interfaces that I can pass strings or arrays into, as well as some functions to take the VMware objects and pass them up as strings or arrays.

Also I got notified from Microsoft that they are doing away with domains.live.com and that if you use that service for mail and so on, it will stop working very soon. So, I decided I would not wait around and moved my wife and daughter over to my Office365 subscription getting their email to work was simple, but my account to forward to the patton-tech.com one…not so much. Hopefully I can figure this out, because just adding that address to my account was not an option.
