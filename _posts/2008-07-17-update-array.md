---
layout: post
title:  "Update Array"
date:   2008-07-17 11:13:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
---
The [UpdateArray](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/procedures/UpdateArray.txt) procedure updates the arrLabs array. The purpose of this procedure is to generate a tallies that will be used to in a report to display lab usage. This procedure depends on the arrLabs array being declared globally as well as the GetProp function being available.

This procedure accepts three parameters:

strADSPath
intArrIndex
strLab
strADSPath
This is the LDAP URL of the object we are tallying.

intArrIndex
There are 8 labs that we are responsible for, so the intArrIndex is the first element in the array that corresponds to a particular lab.

strlab
This is a value that is stored in the first column of the array index.
