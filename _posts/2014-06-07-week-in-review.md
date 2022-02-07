---
layout: post
title:  "Week In Review: 06/01/2014"
date:   2014-06-07 12:02:00 -0600
categories: blog
tags: June 2014 KU IT Scripting Review
---
Another very productive week! Spent a lot of time on Operations Manager, and getting the Low-Privilege SQL Monitoring to work. There appears to be a problem with how the MP calculates PLE and is using data and advice that is now about 10yrs old, so I disabled that monitor.

We have about 20 SQL servers that are directly under our control, so trying to get those setup manually would have been painful. So I worked up a nice little [SQL PowerShell module](https://github.com/mod-posh/SqlModule) for automating some of that for me. Considering the number of servers in total we have, that code is really going to help out.

I didn’t spend all my time in Ops though, I did do a lot of Orchestrator this week. It’s been so nice having the network setup in such a way as to make this all so easy now. There are still some kinks that I need to work out, but otherwise it’s been really fun. One of things I did this week for Orchestrator was build a [PowerShell module](https://github.com/mod-posh/ScorchModule) for it as well. I talked about that one in my last [article]({% post_url 2014-06-06-system-center-orchestrator-powershell-module %}).
