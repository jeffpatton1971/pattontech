---
layout: post
title:  "Updated New-Printjob script"
date:   2012-04-01 15:14:00 -0600
categories: blog
tags: April 2012 KU SOECS Scripting
comments: false
---
The information I’m going to cover here was previously covered on [TechNet](http://blogs.technet.com/b/wincat/archive/2012/03/21/3449236.aspx). I’m posting this because this morning I came across an error in my [PrintLogger](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/New-PrintJob.ps1) script.To be fair it was an error in the script, there is something else going on. I have created a [thread](http://social.technet.microsoft.com/Forums/en-US/ITCG/thread/29739ec0-b367-4a1c-9d0b-42cbbb6bae36/#e6bf38fa-ce99-41a8-843b-701efd4448c3), but I don’t know if I’ll get much in the way of response, as the only hit on [Google](https://www.google.com/search?q=get-winevent+The+maximum+number+of+replacements+has+been+reached&hl=en#hl=en&sclient=psy-ab&q=get-winevent+%22The+maximum+number+of+replacements+has+been+reached%22&oq=get-winevent+%22The+maximum+number+of+replacements+has+been+reached%22&aq=f&aqi=&aql=1&gs_l=serp.3...5007l5444l0l5805l2l2l0l0l0l0l121l217l1j1l2l0.epsugrst.1.&pbx=1&bav=on.2,or.r_gc.r_pw.r_cp.r_qf.,cf.osb&fp=21ba5269d78bca91&biw=1680&bih=926) for the exact error message is a [German](http://www.cycotec.de/index.php/programmieren/powershell/beispiele/148-winrm-aktivieren) site.

The jist of my problem is that when a job is submitted, I use Get-WinEvent to pull in all the events where the Event ID is 307. This is the job printed event and has all the details for the job that I’m interested in. On a busy server this can be a fairly large list, and while at the time of the error there were only about 2100 entries in the log, it was causing it to fail and not log anything.

The quick fix was to tack on –ErrorAction SilentlyContinue to the Get-WinEvent cmdlet. This allowed the code to continue through the error. Another fix would have been to limit the number of entries returned, but still not terribly accurate. Then I remembered that article I listed up at the top, and that I had been messing around with it.

The idea here is, when EventID 307 occurs to pass to the script the Record ID of the event that originally triggered the task. The [original](http://blogs.technet.com/b/otto/archive/2007/11/09/find-the-event-that-triggered-your-task.aspx) article talks about various ways of displaying this information, since I’m working in PowerShell I was more interested in the second.

The code to add is below, and you can add more entries based on the detailed view of a given event. I’ve not tried any others as all I need is the EventRecordID.

``` dos
 Event/System/Channel
 Event/System/EventRecordID
 Event/System/Level
```

I followed the steps below, with the exception of not using the command-line to create and delete a task. I did this originally but later skipped that part as an import was much more simple.

1. Create a task based event
2. Right click the task and choose to export it
3. Edit the XML file add the code above between the EventTrigger tags, and save
4. Delete the original task
5. Import the XML file and modify the properties for the action

For the start a program action, I will just refer you back to the article, all you need to remember is you will need to add two additional Parameters to your PowerShell script, $EventRecordID and $EventChannel.

> $EventRecordID is the record number of the event that triggered this task
> $EventChannel is the log where the event can be found

There was very little adjustment that needed to be done to the original script. I’ll test it for a day, but in limited testing the updated script produced identical results to the original.

This script is also available on the [TechNet Gallery](http://gallery.technet.microsoft.com/New-PrintJob-2f43062f)(Gallery is no longer active).
