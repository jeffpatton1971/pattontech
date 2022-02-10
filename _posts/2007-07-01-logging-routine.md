---
layout: post
title:  "Logging Routine"
date:   2007-07-01 09:00:00 -0600
categories: blog
tags: July 2007 KU SOECS Scripting
comments: false
---
I created a routine, [LogData](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/utils/LogData.txt),  that is pretty simple, it accepts
two parameters:

intCode
strMessage
I use this routine to log when the script starts and stops as well as a basic error handling routine. When I’m using it to handle errors, the strMessage
variable is usually a combination of Err.Number, Err.Description as well as text that might say what I wanted to have happen when the error occurred.

intCode
This is an integer between 0 and 4 that determines the icon that will appear in the Application log on a Windows based computer.

0 = Success
1 = Error
2 = Warning
3 = Information
strMessage
This is a string variable that holds the body of the message, usually in my scripts this is built in another function.
