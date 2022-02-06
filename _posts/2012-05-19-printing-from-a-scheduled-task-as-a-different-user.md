---
layout: post
title:  "Printing from a Scheduled Task as a different user"
date:   2012-05-19 15:20:00 -0600
categories: blog
tags: May 2012 KU SOECS Scripting
---
It does sound a bit odd, but I’m in the process of moving all the regular monitoring I do to scheduled tasks, and this particular one caused me headaches all afternoon.

I have a [script](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/Update-DPMSpreadSheet.ps1) that I run that will update the [DPM VolumeSizing spreadsheet](http://blogs.technet.com/b/dpm/archive/2010/09/02/new-dpm2010-storage-calculator-links-sep-2010.aspx) that Microsoft put together for System Center Data Protection Manager. It’s a great tool, if you’ve not looked at it and are running DPM you should check it out!

The problem I had was I scheduled this to run as my account and it worked just fine. As soon as I configured this to run as a service account, the script would go, but nothing with Excel worked. I found several threads on Google that mention as much.

I finally found a very nice thread on [Technet](http://social.technet.microsoft.com/Forums/en/winserverpowershell/thread/aede572b-4c1f-4729-bc9d-899fed5fad02), the answer is from a user named JensKalski who recommends creating a desktop folder under systemprofile. I have read this before and it escapes me now where I saw this, but as soon as I created this folder on my server, I got the printout!

YAY! Thanks Jens!
