---
layout: post
title:  "SCOM 2007 R2 and Get-Event"
date:   2013-01-03 11:00:00 -0600
categories: blog
tags: January 2013 KU IT Scripting
comments: false
---
It’s been a very long time since I last posted, the primary reason is most likely laziness on my part and secondly I’ve not had a lot to write about. Recently I’ve been messing around with Orchestrator and automation as a means of passing information off to Zenoss. On the face of it, it seemed a rather trivial task, but it took much longer than I anticipated.
The first go round with this was a very simple runbook. It had two activities, Monitor Alert and Run .Net Script. The Monitor Alert activity was configured to look for alerts that were not Information alerts. Once an alert occurred that met that criteria it was passed off to the Run .Net Script. The Run .Net Script activity simply created a simple log entry with PowerShell.

``` powershell
New-EventLog -LogName 'SCOM Alerts' -Source Category
Write-EventLog -LogName 'SCOM Alerts' -Source Category -EntryType Severity -EventId 1 -Message Name
```

Note : I didn’t include all the gibberish typically seen when copying a runbook into notepad, so you can assume that Category, Severity and Name are prefixed by a big nasty GUID.

The first hurdle I had to get around was creating new sources. Since I didn’t know in advance what they would be, it seemed to me it would be easier to have them created programmatically. That’s what the first line does, but the context under which this runs didn’t have the ability to do that. So I created a group and added the service account to that group, and then added that group to the local Administrators group on the server. Finally I needed to disable UAC which was preventing this from happening, if someone has a better way of doing this I’m all ears.

The nice part about this stage is I was able to get some alerts generated and have them show up in the newly created log. For testing I picked a server that I was monitoring and then stopped the HealthService service. This would generate a failed heartbeat alert similar to a computer going offline unexpectedly. With some sample log entries I was able to configure the Zenoss server to pull in the specific log and start generating alerts with Zenoss.

While this worked well enough to get started I wasn’t satisfied with the quality of the data being returned. Specifically I noted that while some alerts contained the name of the computer with the problem, not all did. Looking at the data returned by the Monitor Alert activity it didn’t seem to me I was getting as many of the details as I needed.

So I decided that some remoting might do the trick for me. With remoting I’m able to use the

Links

* [Using SCOM PowerShell Snap-in](http://blog.tyang.org/2012/05/09/using-scom-powershell-snap-in-and-sdk-client-with-a-powershell-remote-session/)
* [CredSSP](http://blogs.msdn.com/b/powershell/archive/2008/06/05/credssp-for-second-hop-remoting-part-i-domain-account.aspx)
* [Use PowerShell to remote to Root Management Server](http://blogs.technet.com/b/stefan_stranger/archive/2010/11/02/using-powershell-remoting-to-connect-to-opsmgr-root-management-server-and-use-the-opsmgr-cmdlets.aspx)
* [Resolve Open Alerts](http://blogs.technet.com/b/jonathanalmquist/archive/2009/03/19/resolve-all-open-alerts-generated-by-specific-agent.aspx)
* [SystemCentral.com](http://www.systemcentercentral.com/BlogDetails/tabid/143/IndexID/70177/Default.aspx)

My thread

* [Using Get-Agent and Get-Alert](http://social.technet.microsoft.com/Forums/en-US/operationsmanagergeneral/thread/360f3a42-9153-4e2e-b060-73740e8ffe4f/#360f3a42-9153-4e2e-b060-73740e8ffe4f)
