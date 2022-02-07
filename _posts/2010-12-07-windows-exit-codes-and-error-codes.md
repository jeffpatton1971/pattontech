---
layout: post
title:  "Windows Exit Codes and Error Codes"
date:   2010-12-07 19:34:00 -0600
categories: blog
tags: December 2010 KU SOECS
---
So historically we have rebooted the lab computers once a week. The reason for doing this originally was to clear our local profile data, and empty the temp and scratch folders. In order to do this Carson created a script, that did some checking on the computers prior to rebooting them. His script relied on WMI, which for us has not always been 100% reliable. Recently he realized that due to the number of computers we currently manage in the labs, and the time-offset he was using and our continued WMI problems, his script would never finish.

So today I decided to write one. Carson and I both figured it would be easier to just use shutdown.exe which is built into Windows for this. Our problem was that it doesn’t have a parameter to handle a list of computers. So we decided we could ask AD for a list of computers in a given OU and then pass those names to shutdown and let it do it’s magic. So that’s what I wrote, [RebootComputers.vbs](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/RebootComputers.vbs), a simple script that queries ActiveDirectory for computers and then using the shutdown command reboots them.

You might be asking yourself, what does this have to do with Exit Codes? Well, I wanted to have some nice error handling and the Run method of the Shell object provides a way to get the error stream. If you look at the following line of code:

> intReturnCode = objShell.Run(strShutdownCMD,1,vbTrue)

The Run method has three parameters, the first parameter (strShutdownCMD) is the command we wish to run. The second parameter (1), is the window style the command is run in. Finally the third parameter (vbTrue), WaitOnReturn. This value is false by default, so setting it to true allows you to capture that code. Now, I didn’t know all the codes that would be returned from the command, but after some diligent Googling, I found something better than what I was originally looking for, a post on the Symantec connect site called [Windows System Error Codes (exit codes) Description](https://community.broadcom.com/symantecenterprise/communities/community-home/librarydocuments/viewdocument?DocumentKey=82da0cae-8efa-4830-bb32-7f52f0a22402&CommunityKey=ef59d715-7ea1-41c6-97f3-dd1bcc10d0c3&tab=librarydocuments)(Moved to Broadcom at some point). This appears to be quite the list of codes returned from Windows, and the really nice thing is that they appear to be either current, or Microsoft has never really changed them. Being the geek that I am I copied that list into a .csv and have placed it on my [server]({% link error-codes.md %}) every now and again I need access to that information.

I’m sure that this information is also located somewhere on the Microsoft site, but at least it’s documented someplace else.

Enjoy!
