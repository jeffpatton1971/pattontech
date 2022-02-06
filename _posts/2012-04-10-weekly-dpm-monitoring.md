---
layout: post
title:  "Weekly DPM Monitoring"
date:   2012-04-01 15:12:00 -0600
categories: blog
tags: April 2012 KU SOECS Scripting
---
Part of my responsibility is handling storage. This includes allocating, deallocating, backing up and restoring. Now we’ve been using DPM for quite some time and are currently running on DPM 2010. Since this past summer I have personally come to peace with the fact that my users don’t know what the delete key is, so I have set some things in place to make it easy for me to monitor overall usage of storage for the School.

Since storage is always increasing, three weeks ago I decided that I would start to regularly monitor the used space on the file servers and update DPM accordingly. For that I used the [DPM sizing tool](http://blogs.technet.com/b/dpm/archive/2010/09/02/new-dpm2010-storage-calculator-links-sep-2010.aspx), it’s a wonderful set of spreadsheets and scripts and if you haven’t played with them, you should!

What I love most about this tool is that you can just type in the used space of a given volume and it will calculate, based on various settings, the new size of the Replica Volume and Recovery Point Volume. So, for the past three weeks I’ve been manually opening up the spreadsheet, firing up RDP, connecting to my server and running [Get-PSDrive](http://technet.microsoft.com/en-us/library/dd315263.aspx) from inside PowerShell.

For whatever reason, today I decided that enough was enough and to automate this for myself. After all I get regular updates from my file server when it runs out of space, so I can add more why can’t I have something similar for DPM? That’s how the [Update-DPMSpreadSheet.ps1](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/Update-DPMSpreadSheet.ps1) script was born.

The idea is pretty simple, for each file server get a list of drives and the amount of used space in GB. So I created a [scriptblock](http://technet.microsoft.com/en-us/library/dd315277.aspx) that gives me the bits of information I require.

```powershell
Get-PSDrive -PSProvider FileSystem |Select-Object -Property Name, @{Label='Used';Expression={$_.Used /1gb}}
```

I use [Invoke-Command](http://technet.microsoft.com/en-us/library/dd347578.aspx) and pass it in a session object and the above scriptblock and capture the results. When I’m done I close out of my session with [Remove-PSSession](http://technet.microsoft.com/en-us/library/dd315404.aspx) that way I don’t consume too many resources.

> There is a max limit on the number of concurrent sessions an account can have open. This default is 5, and can be modified as needed. Please see the following [article](http://blogs.msdn.com/b/powershell/archive/2010/05/03/configuring-wsman-limits.aspx) for details on how to do this.

Once I have all that data I create a new instance of [Excel](http://blogs.technet.com/b/heyscriptingguy/archive/tags/scripting+guy_2100_/microsoft+excel/), open the DPM Sizing Tool spreadsheet, and set my worksheet to the DPM File Volume sheet. I use the Volume Identification column to match up against the list of drives that are returned from my servers. As of v3.3 of this tool that column is column D. Once I find the current drive in the spreadsheet I hop over one column and update the value of the Used space in GB column (Column E as of v3.3).

If there are any errors along the way, I log them to the Application log and close out of everything.

I had thought about creating a scheduled job to have this run every Monday, but seeing as how my computer might be off or something I took the low-tech route. I updated my [$PROFILE](http://msdn.microsoft.com/en-us/library/windows/desktop/bb613488(v=vs.85).aspx) with the following chunk of code.

``` powershell
if ((Get-Date).dayofweek -eq 'Monday')
{
    C:scriptspowershellproductionUpdate-DPMSpreadSheet.ps1
    Invoke-Item 'C:UsersjspattonSyncStuffDPMvolumeSizing v3.3DPMvolumeSizing.xlsx'
    }
```

Hopefully it’s pretty straightforward, if today is Monday, run the [Update-DPMSpreadSheet.ps1](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/Update-DPMSpreadSheet.ps1) script, and then open it up in Excel.

I have also uploaded a version of this script to the [Technet Gallery](http://gallery.technet.microsoft.com/Update-DPMSpreadSheetps1-94cced9c)(Gallery is no longer active).
