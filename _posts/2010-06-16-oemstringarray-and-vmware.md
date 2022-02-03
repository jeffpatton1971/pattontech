---
layout: post
title:  "OEMStringArray and VMware"
date:   2010-06-16 19:15:00 -0600
categories: blog
tags: June 2010 KU SOECS Scripting
---
OEMStringArray as defined by Microsoft will contain:

>List of free-form strings that an OEM defines. For example, an OEM defines the part numbers for system reference documents, manufacturer contact information, and so on.

I was looking for a simple thing that would let me know the number of stored user profiles on the system. It’s something that had come up before and there was a property that looked promising, Win32_OperatingSystem.NumberOfUsers, but it wasn’t. So I started poking around looking for stuff.

When the following code is run against a VMWare machine it returns the following tidbit of information.

``` visualbasic
strComputer = “Vmware-pc”
Set objWMIService = GetOBject(“winmgmts:\” & strComputer & “rootCIMV2”)
Set colItems = objWMIService.ExecQuery(“SELECT * FROM Win32_ComputerSystem”,,48)
For Each objItem in colItems
Wscript.Echo “OEMStringArray: ” & Join(objItem.OEMStringArray)
Next
```

OEMStringArray: [MS_VM_CERT/SHA1/lotsoflettersandnumbers],Welcome to the Virtual Machine

I don’t know about you, but I couldn’t stop singing [Pink Floyd – Welcome to the Machine](http://en.wikipedia.org/wiki/Welcome_to_the_Machine)!
