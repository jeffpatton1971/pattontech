---
layout: post
title:  "Windows Eventlog Management Part 1"
date:   2012-04-01 15:10:00 -0600
categories: blog
tags: April 2012 KU SOECS Scripting MissingImages
---
USING EVENT TRIGGERS AND EVENT FORWARDING TO GET WHAT YOU WANT FROM THE EVENT SUBSYSTEM
Event logs are horrible, and depending on which log you’re looking at they could be even more horrible!

![EventLogs]()

Seriously though, I shouldn’t say they are horrible, there is just so much that sometimes things get lost in the chatter. Prior to Windows 2008 there were only 3 logs that we had to worry about Application, System and the dreaded Security log. With the Release of Windows Server 2008, countless other logs have been added, Event ID’s have been changed, and the underlying services that report events potentially have their very own log to write to, that is of course if it’s been enabled!

What do you want to know?

* Low disk space
* Invalid logon attempts
* Network outage
* Service failures
* Time synchronization issues

The answer to this question depends on so many things, there are literally no wrong answers when it comes to event monitoring. The key is to start looking! You’re never going to know what it is you want to focus your attention on, if you never open up the console.

![EventViewer]()

The nice thing about the Event Viewer is that we can apply filtering so instead of seeing a year’s worth of entries, narrow it down to this month, or this week, or just today. While we would all love to see the friendly blue icon that lets us know the server is happy, the fun only begins when we start looking at the Warning and Error entries. You might be surprised but your log could dwindle from 36,981 entries to a paltry 224!

Granted the resultant log looks way more scary because it’s filled with yellow and red icons, but this view is way more interesting in terms of troubleshooting and monitoring.

Be familiar with the log

* Are Error and Warning entries all I need to worry about?
* Is it ok that the Operational log I’m looking at is empty?
* Do I need to be concerned about each Error or Warning entry?

This goes back to what I said earlier about opening up the console. While it’s ok to start out with a filter for Error and Warning entries, not all logs report problems as an Error or Warning. When you drill down into the Applications and Services logs, often times they are filled with Information entries, and that entry may let you know something either did or didn’t happen. So you need to be familiar with your log and know what events are things you want to be aware of.

For example, the DHCP Filtering log reports MAC denies as an Information entry, if you were filtering for Error and Warning entries you would never see that, assuming you care.

As I mentioned earlier, not all of those new logs are enabled by default. If you have a print server and want to know who is printing, when you open up the Print Server Operational log, it’s empty. Does that mean that nobody is printing? Perhaps, but since the log is off by default, you may want to enable it before you make your decision. Once you become familiar with whatever log you’re looking at, you’ll be able to determine if those red error entries are really something to worry about.

For example, once you have enabled that Print Server Operational log, you may see a recurring Error event, Event ID 812. In our environment our user accounts reside in an external domain, and that entry is indicating more or less a false-positive. The error is the spool file was unable to be deleted, access is denied. The reality is that the spool file did in fact get deleted, so this particular error I don’t need to worry about.

But when I first encountered it, I was concerned about it. I searched for that Event ID on the TechNet site, asked questions in the forums, and searched Google. Only when I satisfied myself that there was nothing I could do to keep this error from occurring, and that the error really wasn’t an error did I decide to ignore it.

Now that we’re comfortable with our logs let look at some fun things to do with them.

Part 1

[Part 2](2012-04-01-windows-eventlog-management-part2.md)
