---
layout: post
title:  "Script Details"
date:   2008-07-02 11:12:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
---
I just wrote the [ScriptDetails](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/functions/ScriptDetails.txt) function today, I wanted better information in the Application Log regarding what script executed, where was it located, and who executed it. This information is then passed back to the LogData procedure to be input into the Application Log.

The functions accepts one parameter:

strComputer
strComputer
This function takes advantage of the Wscript object to return the name of the script in memory, and its fully qualified path. Then it uses WMI to connect to the computer, it uses the value in strComputer to return the username of the user currently running this script.