---
layout: post
title:  "Get recent events from servers"
date:   2012-04-01 15:17:00 -0600
categories: blog
tags: April 2012 KU SOECS Scripting
comments: false
---
I’ve been working with Microsoft on an issue that I am having with my DPM server. We have been doing some fairly intense logging, and today I enable several performance counters in an attempt to ascertain if something external is triggering this issue.

Along those lines I thought it would be cool to get a list of log entries from two hours before the event occurs. The event I’m tracking is DPM 3101, Volume Missing. We have seen that during a regular backup something happens and then DPM stops with the message that the disk I’m backing up to is no longer connected.

I’ve started a [thread](http://social.technet.microsoft.com/Forums/en-US/dataprotectionmanager/thread/061a8ac3-ac01-461a-b75b-40cdb3683e23/) and have participated in several other threads on the forums about this issue.

At any rate, I decided that I would write a [script](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/Get-RecentEvents.ps1) that would grab up all the events from my DPM server and my two file servers, that I’m backing up. The hope is that maybe something interesting will be logged.

Why the two hours? Well, it’s silly, but I’ve noticed that two hours seems to be significant in the timeline of how these things are happening.

The script is also available on the [TechNet Gallery](http://gallery.technet.microsoft.com/Get-RecentEventsps1-940ec83b)(Gallery is no longer active).
