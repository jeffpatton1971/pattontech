---
layout: post
title:  "Build Report"
date:   2008-07-01 11:05:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
comments: false
---
The [BuildReport](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/procedures/BuildReport.txt) procedure is a basic file creation script. It verifies that the folder that is passed in exists before processing anything. I could have made the script create the folder if it didn’t exist, but in our environment a given user may not be able to create a directory. This way it logs the error in the application log for later review.

This procedure accepts two parameters:

strFileName
strFilePath
strFileName
This is the filename and extension of the file you are creating.

strFilePath
This is the path to the file including drive letter assignment. If the folder doesn’t exist, the procedure logs an error and exits the routine.
