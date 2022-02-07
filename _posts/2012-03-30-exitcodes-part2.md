---
layout: post
title:  "ExitCodes Part 2"
date:   2012-03-30 15:09:00 -0600
categories: blog
tags: March 2012 KU SOECS Scripting
---
So, [yesterday]({% post_url 2012-03-20-powershell-new-adinventory-script %}) I mentioned that I re-wrote the inventory [script](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/New-AdInventory.ps1). Today I decided to re-write the [reboot script](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/Reboot-Computer.ps1). The idea behind the script is that once a week we bounce all the lab computers. We do this for various reasons, but since I’m in the mood decided today was the day to tackle that problem.

The [last time]({% post_url 2010-12-07-windows-exit-codes-and-error-codes %}) I talked about this, I got a little off the beaten path hunting down all possible exit codes for the shutdown.exe command. While not wrapped around the axles this time, I did have to figure out how to deal with it.

The nice thing about PowerShell is that when running a command you have access to $LASTEXITCODE. This contains exactly what you think it contains, the number of the return code from the command-line program. Before I get to far ahead I do want to mention that when last I wrote about exit codes I found them on the [Symantec site](http://www.symantec.com/connect/articles/windows-system-error-codes-exit-codes-description) (still works). Today I found an archived newsgroup that had a link to the [MSDN](https://docs.microsoft.com/en-us/windows/win32/debug/system-error-codes) site, so I’ll put that here.

Ok, so I decided since I was re-writing this thing I wanted to be a little more accurate in my reporting of errors encountered. Now it was impossible for me to find what error codes are returned from shutdown.exe, most likely because it could be any number. So then I started looking at how I could get what it was using $LASTEXITCODE.

Buried deep in my brain I remembered that there was a net command that would give you a text version of the number.

``` dos
net helpmsg 53

The network path was not found.
```

That seemed perfect, What happens if I use $LASTEXITCODE

``` dos
net helpmsg $LASTEXITCODE

The operation completed successfully.
```

BRILLIANT! This was perfect, I decided to store the result in a variable and then write it out. The only problem, really more of a hassle, was that it returns a string array.

``` powershell
$result = (& net helpmsg $LASTEXITCODE)

$Result.Count

3
```

After some poking around I realized that the first row is blank, the second row contains the message and that the remaining rows were empty. So in my case, one line padded top and bottom with empty rows. Then I began to wonder, are all the messages one-liners? So I wrote up a little routine to display all the messages, I’ll give you the final version of it.

``` powershell
$ExitCodes = (0..15818)
foreach ($ExitCode in $ExitCodes)
{
    try
    {
        $ErrorActionPreference = 'SilentlyContinue'
        (& net helpmsg $ExitCode)[1]
        }
    catch
    {}
    }
```

You might be asking why am I stopping at 15818? If you visited the link I gave you earlier you would have noticed that the codes ran higher than that. In fact the last page of that list is [System Error Codes (12000-15999)](https://docs.microsoft.com/en-us/windows/win32/debug/system-error-codes--12000-15999-). Well if you scroll to the bottom of that page, you will note it stops at the above listed 15818. Now I don’t know why, but I figured why go any higher right? Well, I did and there isn’t anything there.

This script is pretty straightforward, it loops through each number and passes it to net helpmsg. All I did then was just ask for the second row [1] of that returned object. While I didn’t count all the returned messages, there were a lot, and for my situation, the one line on the second row was plenty for me.

The script can also be downloaded from [TechNet](http://gallery.technet.microsoft.com/Reboot-Computerps1-5d530000)(Gallery is no longer active).
