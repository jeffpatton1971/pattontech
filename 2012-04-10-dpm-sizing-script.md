---
layout: post
title:  "DPM Sizing Script"
date:   2011-04-01 15:13:00 -0600
categories: blog
tags: April 2012 KU SOECS Scripting
---
[Yesterday](2012-04-10-weekly-dpm-monitoring.md) I told you how I had decided to automate a portion of my DPM routine. As usual this got the fires burning and a second script was born. I would have told you about it yesterday but I wanted to make the appearance of doing actual work 😉

So today I give you the [Get-DPMSizingValues.ps1](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/Get-DPMSizingValues.ps1) script. This is basically the portion of the DPM Sizing tool that I use regularly, the part that deals with file servers. I must say I’m rather proud of it as it worked out better than I thought it would. It uses some of the same basic stuff as the previous script, which was nice for me.

My [Get-PSDrive](http://technet.microsoft.com/en-us/library/dd315263.aspx) statement is a little different. I noticed when I ran this against my Windows 7 machine I had a lot of cruft I didn’t care about, so you’ll note the [Where-Object](http://technet.microsoft.com/en-us/library/dd315399.aspx) bit. That filters out any results that have less than or no used space.

``` powershell
Get-PSDrive -PSProvider FileSystem |Where-Object {$_.Used -gt 0} |Select-Object -Property Name, @{Label='Used';Expression={$_.Used /1gb}}
```

The nitty gritty part of it uses the same formula found in the spreadsheet. Now, there are some values that are hard-coded as these are direct from Microsoft and I don’t really know what they mean as they have not been terribly forthcoming about it, or my fu is just not working for me today.

``` powershell
if (($ReplicaOverheadFactor/100) -gt 1)
{
    $ReplicaVolume = $VolumeIdentifier.Used * ($ReplicaOverheadFactor/100)
    }
else
{
    $ReplicaVolume = $VolumeIdentifier.Used * 1.5
    }

if ($VolumeIdentifier.Used -gt 0)
{
    $ShadowCopyVolume = ($VolumeIdentifier.Used * $RetentionRange * ($DataChange/100)) + (1600/1024)
    }
```

So I just found a bug while writing this and fixed it, turned out I forgot to convert the ReplicaOverheadFactor into a fraction in that first test. Oh well, it’s working now which is good. At any rate, that is the heart of the script, that gets looped through for every drive that has used space. I had thought about not doing the second test, since my scriptblock actually shouldn’t return any volumes that have zero used space, but what the heck, it doesn’t hurt anything.

The resulting output is pretty nifty, I would imagine you could potentially pipe this into dpm cmdlet, but I haven’t verified that. If someone needs it I’ll look into doing that but for now, it’s a very nice little reporting tool that will give you calculated values for Replica Volumes and ShadowCopy Volumes.

``` powershell
Name            : C
UsedSpace       : 44.3877143859863
Retention       : 7
Replica         : 53.2652572631836
ShadowCopy      : 32.6339000701904
DataChange      : 10
ReplicaOverhead : 120
```

There is also a version on the [Technet Gallery](http://gallery.technet.microsoft.com/Get-DPMSizingValuesps1-cdbfe9f9)(Gallery is no longer active).
