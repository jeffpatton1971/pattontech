---
layout: post
title:  "Week In Review: 06/15/2014"
date:   2014-06-22 12:04:00 -0600
categories: blog
tags: June 2014 KU IT Scripting Review
comments: false
---
It’s time for another exciting edition of WIR! This week was filled with updates! Rolled updates to our Domain Controllers and one of them took nearly two hours to come back from a reboot! Normally not a big deal, but when your 30mi away…a little stressful! I also rebuilt my work laptop this week, earlier this year I had done something stupid with an external drive and wound up with Windows installed on Partition 2, on a disk with just one partition! Needless to say, rebooting my laptop didn’t happen all that often at all!

Speaking of Active Directory Domains, we are moving ever closer to having just one domain on campus. The internal private Edwards domain went away this week! It’s always just a little nerve wracking when running through dcpromo to remove stuff, but it went well. Didn’t appear to leave any unsightly meta data floating around AD!

Also spent a fair amount of time talking with the guys at Edwards to go over how they image machines. They routinely call us to have a workstation DNS entry removed, and needless to say it’s a little annoying. They ought to be able to do this themselves, but since it’s not their DNS they don’t have rights. Not to mention they way they do their image is a little different.

This is how it goes, a user is up for a new computer. In an effort to minimize the inconvenience this can sometimes to be, they image the new computer, load their software, and finally join it to the domain. This last part is what gets them, they tack on a “-1” to the new workstation name. Normally not a big deal, but the last part is where it gets hairy.

The new workstation is delivered to the user, the old workstation is unjoined from the domain, the new computer is renamed to the old computer name…and boom. Sometimes this works (they say) but I can’t imagine how. So, the first comment was hey, how about using service tags, or mac addresses to identify these machines uniquely, then you will never get hit with this issue. Nope, they like usernames as computernames, it makes it easy to correlate user to workstation. Apparently it’s too difficult to track that down in SCCM? Not likely, but oh well.

So, what to do, well we could just have them call every time, but that’s a hassle, not to mention there’s no code involved! My solution, create an Orchestrator runbook, that is provided a computername. With that information it scrubs AD and removes the DNS entry as well. This Runbook would run in the context of a service account that has rights to do this. They would simply login to it with their admin account, we would use their group information to verify that the computer they want removed lives in their OU and then remove it and the DNS entry. If it doesn’t live in their OU it fails. Sounds elegant to me 😉

A final solution, which will take much longer to implement, will be an appliance from BlueCat that sits between AD DNS and Proteus DNS. This appliance will use the Proteus web service and the MS RPC to translate information between AD and DNS. This will get us to a very similar place as my Runbook idea, but the one advantage is this will also get us to a place where we can pull our AD DNS out of the public facing DNS, effectively hiding thousands of servers and workstations.

Another fun one that happened, you can’t push the ops client to a Domain Controller using SCCM Client Push. If someone tells you they can, they are lying to your face! I’m going to write up a post, but the short of it is, Client Push relies on a local administrator to work, how do you do that on a Domain Controller?

OH! I also polished off my SQL PowerShell, so I’ll write about that as well. It works pretty well, created some new functions to let me more accurately find SQL Instances, still don’t have a good way to talk to the WID but it’s kicking around in the back of my head.

I also broke Active Directory Certificate Services..see you next week!

Oh, I suppose we should talk about that? So, I’ve been slowly pulling servers out of the old Ops servers and bringing them over to the new. Doing pretty well, 230+ servers in the new and growing, and under 50 in the old. The Domain Controllers got pulled in this week as well as the Certificate servers.

So, I’m working through the alerts, tuning Ops so I only hear what I need to. So, I started getting alerts about ADCS (Active Directory Certificate Services) and started working on that issue. I was seeing errors about the CRL Distribution Point being offline.

As part of the troubleshooting I had already decided to stand up a vhost to hold CRL’s among other things. So I reconfigured the CA to use that, after restarting the service as prompted by Windows, Certificate Services failed to start. The net result here was that the CRL’s were out of date and just needed to be published and then copied to the web location.

The only bit left here is to automate both the publishing and the copying of the files over to the web server. Of course this seems well suited to creating a PowerShell solution, check back later for that!

See you next week!
