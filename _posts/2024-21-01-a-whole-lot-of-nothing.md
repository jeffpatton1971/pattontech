---
layout: post
title:  "A whole lot of nothing, until there was something"
date:   2024-01-21 22:21:00 -0600
categories: blog
tags: January 2024 Development WIR
author: Jeff
comments: true
published: true
---

This week was filled with a lot of meetings, a few unexpected, and at least one was mandatory. I always hated those mandatory meetings, they always went the same way, 5 min of information followed by an eternity of things that hvae no bearing on what I do in my day to day. Beyond meetings, progress was made at work, my service account has been approved and I'll be finally sorting out the last of my deployment work. In an effort to stay productive I have also resurrected a project that I have ignored for a long time. The nice thing is that when I'm stuck waiting on other folks so I can do my job, I can work on this. Looking forward, this work shoud lead me to having a full API to handle the entire process, and possibly provide the ability to define command-line tools as not everyone will want to go through a UI.

My own work has progressed nicely as well, I have now created several [GitHub Actions](https://docs.github.com/en/actions) that should really help me in the long run. As always you can find the code living in the [Mod-Posh Organizatin](https://github.com/mod-posh). I've got 4 Actions down and I think at least 4 more to go, not including things I pull out and create seperate actions for. For example, I'm working on an Action that will setup my environment for building and deploying, part of that required pulling a version from a PowerShell module or a C-Sharp project file. I have moved that into a seperate Action dedicated to just getting the verison, this allows the Action I'm working on to be solely dedicated to that aspect. Basically anything that can be calculated or passed into the Action by some other means should be. The Action should only do the thing it was written to do and not anything beyond that.

As I work through these, I'm beginning to realize that they can also be written to work with [Azure DevOps](https://azure.microsoft.com/en-us/products/devops/) as [Pipeline Templates](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops&pivots=templates-includes). As I do a lot of work in both environments this may lend itself very nicely to that work as well.

Finally, I'm starting to work on a [BuildAutomation](https://github.com/mod-posh/BuildAutomation) project that is akin to the Github Actions I'm working on. But this project is a PowerShell module that will allow me to really pare down my [psake](https://github.com/psake/psake) file. This BuildAutomation module will encapsulate the code in that script now, into easy to use cmdlets. Not everything will be able to be removed, but I'm hopeful that I can pull out a large percentage of that code. I may also spin up a few [PowerShell script modules](https://learn.microsoft.com/en-us/powershell/scripting/developer/module/how-to-write-a-powershell-script-module?view=powershell-7.4) for [BlueSky](https://github.com/bluesky-social/atproto) and [Discord](https://discord.com/developers/docs/reference) for the notifications that I use.

I hope you have a great week ahead, and I'll write more later!
