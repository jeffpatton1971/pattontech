---
layout: post
title:  "WTF Write-Verbose"
date:   2022-09-04 23:30:00 -0600
categories: blog
tags: September 2022 Development
author: Jeff
comments: true
published: true
---
So I've been working on some really fun things at work that tie into some of the projects that I'm personally working on, which I guess is par for the course for me. This past Friday I was struggling with re-writing a PowerShell module to leverage my new C# library. The issue was the output was returning blank data, annd it wasn't readily apparent. It was only after piping through ConvertTo-Json that I was able to even see the issue at all.

```powershell
[
  "",
  {
    "Type": "OS",
    "Caching": "ReadWrite",
    "Sku": "Premium_LRS",
    "SizeGb": 128,
    "WriteAcceleration": false,
    "Lun": 0
  },
  "",
  {
    "Type": "OS",
    "Caching": "ReadWrite",
    "Sku": "Premium_LRS",
    "SizeGb": 128,
    "WriteAcceleration": false,
    "Lun": 1
  }
]
```

The extra blank objects shouldn't be there, and it drove me crazy! I poked at this off and on over the weekend and it wasn't until about 30min ago that I was able to find the offending line of code and remove it.

One of the things I tend to do with my PowerShell is use Write-Verbose pretty librerally. I will kick most anything out for the -verbose switch that way I and you can see what's going on during execution. But I think the issue here is that this line of code was causing the problem.

```powershell
Write-Verbose $Data |out-string
```

That is a parameter that is passed in from the pipeline and I think the simple fact of expanding it out to display it was what was causing the issue. As soon as I removed it the problem went away. I've done this before with parameters without issue, but I'm wondering as this is an object and PowerShell has to expand it before writing it out if that's not the problem. I'll need to dig into that some more to really see, assuming at this point I'm so inclined, it's been a long weekend and I've been chewing on this off and on all weekend.

At any rate a quick post to document a really irritating issue, hope it helps. I should also be pushing out some updates to some of the other code I've been working on so hopefully have a post about that later this week or next.

Happy memorial day!