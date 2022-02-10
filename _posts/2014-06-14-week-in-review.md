---
layout: post
title:  "Week In Review: 06/08/2014"
date:   2014-06-14 12:03:00 -0600
categories: blog
tags: June 2014 KU IT Scripting Review
comments: false
---
Still a lot of programming this week, but like I said before I think anymore that is more the norm than not. We did some interesting Active Directory stuff this week. We had a handful of servers get their AD objects deleted at some point, and we found out about the beginning of this week. Now my guess is these were deleted close to about 3mo ago, and they either rebooted recently or attempted to change their password recently.

About a year or more ago we changed our audit policy and started using Advanced Auditing. We were concerned about user account and group account management, but it turns out we should also have put in computer account management as well. When a computer object is deleted event 4743 is logged in the security log of the domain controller. We searched and couldn’t find that entry anywhere, when I started researching that event is when I found you need to tick the boxes for computer management.

Along those lines we had a similar issue, our admin accounts in our QA domain were disabled, since we do very little auditing at all in there, I enabled the same features so we can see when that happens. When a user account is disabled event 4725 is logged. To go along with both of these events I’m going to update our reporting in Ops on things like this.

While doing all this I found a very nice support [article](http://support.microsoft.com/kb/947226/en-us) listing out the various event id’s and what they mean.

All of the servers that are supposed to report in to System Center Advisor, are now doing so. I feel rather stupid about the issue originally. So my first problem is that I wasn’t patched up to where I needed to be in order to even use the preview, so that was step 1. The next part is where it gets a little fuzzy, I don’t actually recall patching the clients on any of the agents reporting in, yet all 3 domain controllers reported an update agent. Coincidentally all 3 domain controllers were the only servers showing up. After some investigating with the SCA guys from Microsoft they quickly realized I had not patched my agents. So, I must have patched the DC’s, I just don’t recall doing it, hence the stupid.

So the result of this is a working program in SCCM that will patch outdated clients, which is good as my next step in this whole saga is to patch production. It’s either patch, or move over to R2 and currently I’m leaning towards patching. So currently in QA when a server gets discovered the ops client gets pushed down to them, now it will also get patched. Then the only manual part of this process left is to add them to the advisor management pack.

It’s been lots of fun talking with these guys about stuff, I’ve been invited to participate in an SCA board to go over new features and talk about how things work. My recent experiences dealing with some of the internal folks with Microsoft really make me want to work there more.

I’ve done some fun things with PowerShell this week. A new SQL module has been fleshed out and validated against just about all instances of SQL. I’m still having a hard time working with a connectionstring for the Windows Internal Database, but it will come. I’ll most likely write about this module after I’m done with this WIR.

I’ve updated the Orchestrator module. The Start-scoRunboook function worked incredibly well if you only ever had one parameter, as soon as you throw more than one it freaks out. How I originally handled it was dumb, so now the function accepts requires a hashtable object, it then compares the key (property name) field against what the Parameter object returns. This worked out extremely well, again probably a topic for a whole blog post as well.

One last pure PowerShell item is a function that writes functions. It’s not too terribly complicated and I *WILL* post about this later, but basically the idea is that Orchestrator contains Runbooks that perform some action, my module reads those Runbooks in, gets their parameters and allows you the admin to run them. What if we could have a function that would build cmdlets based on that information on the fly…

SharePoint Online! How much fun is it working with UserProfiles in SPO? Well, let me tell you, in order to do anything meaningful it appears you have to access a 10yr old web service that must be ripe for deprecation but has been forgotten about! I’d really like to get some more information direct from Microsoft about that. At any rate, I’ve got some POC code that will allow me to programmatically populate a SharePoint user’s profile with information that we glean from another source. The next step down this rabbit hole is using a 7yr old SDK (Office Server 2007) to see if I can create UserProfile subtypes! I’ve got some examples of how this works, but I’ve not written anything up yet to see if it will go, fun times ahead!

Keeping in line with the SharePoint Online topic, creating admin cloud accounts. So we have an Azure subscription that allows us to get into Azure AD for our tenant, which isn’t anything special. If you have an Office365 subscription, you can create an Azure account, hook the two together and boom…Azure AD! So I created an admin account for me, and one of the other guys on the project. After that I enabled the Multi-Factor Authentication on these two accounts. Now, when I login with my admin account, I receive a txt message with a verification code. So we have looked at this as THE way to secure access to these accounts as we begin to think about the cloud.

With that out of the way, I can talk about the Orchestration. I’ve created a Runbook that will connect to our tenant and provision a user. This came out of the Provisioning project for the larger SPO project. This code takes a single parameter, samaccountname, and then provisions that user in o365 with the appropriate licensing. There are two differences between an o365 user and a cloud admin. The first is licensing, a cloud admin gets none by default (our design), second the all important UPN, user@tenant.onmicrosoft.com. The idea is these accounts live solely in the cloud, and are used specifically for administering cloud things. I have a couple modification in mind, first I need to populate the AlternateAddresses field, as well as the MobilePhone field. Then I need to see if I can enable MFA in Azure for these accounts automatically.

Lots of Orchestrator this week, but now that I’m ready and the network is ready it’s time to start working on Orchestrating Windows Updates. I’ve started a rough draft of that at the moment:

1. basically get a list of servers (or service)
2. for each server start maintenance mode (ops and Zenoss)
3. get the applicable updates (SCCM perhaps)
4. apply the updates
5. reboot if needed
6. make sure the server is back online
7. check if required services are running
8. leave maintenance mode
9. and move on to the next server

If one server in a group fails then we need to stop the update process and throw an alert in ops and Zenoss. This will prevent an entire service from going offline if the updates cause an issue.
