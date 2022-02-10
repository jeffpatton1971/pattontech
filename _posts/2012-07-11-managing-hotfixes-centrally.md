---
layout: post
title:  "Managing Hotfixes Centrally"
date:   2012-07-11 15:22:00 -0600
categories: blog
tags: June 2012 KU IT Scripting
comments: false
---
Historically I’ve not paid much attention to hotfixes or patches, but lately I’ve decided that I need to change that aspect of my management. I used to create a folder on the server that had the problem, copy the patch down and then apply it. As I become responsible for more and more servers, I decided that I needed a slightly better way to handle that. Also, since I tend to do pretty much anything in PowerShell I figured I needed to write some functions to do it for me. The result was the [QfeLibrary.ps1](https://github.com/mod-posh/QfeLibrary/blob/main/QfeLibrary.psm1), it contains several functions that can be used to help you manage your hotfixes.

I thought about how I go about patching a system, and then tried to condense that down into code. The first step is identifying when I have a problem, I usually use [System Center Operations Manager](http://technet.microsoft.com/en-us/systemcenter/bb498233) to help identify when I have a problem. Once I find a problem I start researching on the internet for a solution. When I run into an issue that needs a hotfix, I’ll open the [Microsoft Support](http://support.microsoft.com/search/default.aspx?query=kbhotfix&catalog=LCID%3D1033&mode=r) page for it, and figure out if I need this patch.

A lot of patches seem to modify files, and usually the KB page will tell you what the previous version of the file was, and what the new version should be. To be honest this is where the idea came from, I was using [Get-Item](http://technet.microsoft.com/library/ee176851.aspx) to retrieve the [VersionInfo](http://msdn.microsoft.com/en-us/library/system.diagnostics.fileversioninfo.aspx) of a particular file and thought I should write a script for this, and then decided a script would be too much. So of course the next thought in the progression is write a pile of code with several functions…oh well.

The premise behind this is a central location to store the hotfixes. This can be a local folder or a file share, the preference would be a file share that is accessible from your servers. Then a function that will create an XML file that will hold the test. Then we’ll need a function to run that test to make sure that it’s applicable. Then a function to list all the available hotfixes in the file share. Finally a couple of functions to install and uninstall the patch, and finally a few ancillary functions to clean up, set the file share as a global variable, and perhaps one to view the URL for a given hotfix.

The first function I wanted was something that would output an object that had the Url of the article, the actual KB article number, and a way to test it. This was actually rather difficult for me, I had no problem creating an object that had a property that was a scriptblock, the problem was when I exported that to an XML file it turned into a string. In searching how to convert a string into a scriptblock I found this lovely [article](http://learningpcs.blogspot.com/2009/12/powershell-dynamically-create-script.html) from three years ago.

Once I had that straightened out everything else fell into place nicely. A function to run the test that was stored in the XML file and a pair of functions to install and uninstall a patch. The next thing I needed was a function to get a list of hotfixes available for the local system.

The function to list available hotfixes is a little complex, it will list all the hotfixes that are available based on the OS of the target system. Optionally you can get a listing of all hotfixes that are available, additionally you can then download those hotfixes. No real magic here, when I create the XML for the hotfix I use the caption property of the [Win32_OperatingSystem](http://learningpcs.blogspot.com/2009/12/powershell-dynamically-create-script.html) class. I use this in the function to list hotfixes as my comparison.

QFE WORKFLOW
The first thing that we need to do is define where we will store our hotfix files, this is done with the Set-QfeServer function. Once we’ve done that we need to figure out what the test will be, once we have the test figured out we run the New-Qfepatch function and provide the URL, KB article number, OS, processor architecture, the test and the answer. We will run this for each SKU that we are responsible for. Copy the hotfix file manually to our QfeServer location.

From the target computer, we run the Get-Qfelist function to get a list of hotfixes that are available for our OS. We can run the Get-Qfe function with the online switch to view the support article. We can run the Test-QfePatch function on the target OS to see if it applies. If we pass the download switch to Get-QfeList we will download all hotfixes for the target OS. Finally we can Install-QfePatch on the target OS.

When we’re all done, we can review the logs stored locally if we need to, and finally Clear-QfeLocalStore to zip up the installed hotfixes and log files into a time-stamped zip file.

1. Set-QfeServer -QfeServer \serversharehotfixes
2. New-QfePatch -URL -KB -OS -Arch -Qfefilename -Test -Answer -QfeServer
3. Get-QfeList -QfeServer
4. Get-Qfe -QfeServer -QfeId -Online
5. Test-QfePatch -QfeId -QfeServer
6. Get-QfeList -QfeServer -Download
7. Install-QfePatch -QfeFilename
8. Clear-QfeLocalStore

This function can be downloaded from the [Mod-Posh](https://github.com/mod-posh/QfeLibrary/blob/main/QfeLibrary.psm1) site, or from [Technet](http://gallery.technet.microsoft.com/QfeLibrary-f50762da)(Gallery is no longer active).
