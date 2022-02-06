---
layout: post
title:  "System Center Orchestrator Module"
date:   2014-06-06 12:01:00 -0600
categories: blog
tags: June 2014 KU IT Scripting
---
This is one I’ve had on the back burner for a while, so yesterday morning I roughed up the basic framework for a PowerShell module. I have a few Runbooks at work, that it would be super cool to just run from PowerShell, and since lately I’ve been all up in the web services this was as good a time as any.

The Get cmdlets were all pretty simple, in fact there is really only one that does any real work Get-scoWebFeed. I probably could have used Invoke-WebService, but that’s no fun so I used .Net to make my own, and it’s really pretty simple. I just go ask the Orchestrator server (on a specially crafted url) to spit out the xml, then I just return it.

The individual functions for getting Runbooks, Jobs and Activities handle building the special URL,which isn’t really special as much as it is specific.

The Start-Runbook was the most complicated, I actually borrowed some code from [MSDN](http://msdn.microsoft.com/en-us/library/hh921685.aspx), and another guys blog ([Part 1](http://jmattivi.blogspot.com/2012/06/scorch-powershell-to-start-runbook-part.html), [Part 2](http://jmattivi.blogspot.com/2012/06/scorch-powershell-to-start-runbook-part_02.html)) to build mine. Turns out some of the xml you have to build to send up has to go in a certain way. I need to adjust my code to handle Runbooks with Parameters, but right now it’s good for what I need it do.

You can find the up to the minute code on [GitHub](https://github.com/mod-posh/ScorchModule), or you can find it in the [TechNet Gallery](http://gallery.technet.microsoft.com/SCOrchestratorManagementpsm-5c16093f)(Gallery is no longer active).
