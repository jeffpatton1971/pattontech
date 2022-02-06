---
layout: post
title:  "Setspn.exe Wrapper"
date:   2013-07-13 11:03:00 -0600
categories: blog
tags: July 2013 KU IT
---
It’s been a while since I’ve posted anything, so I thought I would post about setspn, because you know, it’s so awesome right?

So one of the projects I’ve been working on lately is the upgrade to SCCM 2012. Outside of a few things it’s been going very well. We ran into an issue though when we rolled out the production server. Maybe I’ll write a post for that, needless to say part of the solution is SPN’s.

Now, I’m no stranger to this tool, but needless to say it leaves a LOT to be desired. Especially when we consider this came out for Windows Server 2003! So, since I had to do some work with SPN’s I decided I needed a PowerShell way of handling this.

There is really only handful of things we ever need setspn for, add an spn to an object, get an spn for an object, remove an spn from an object, find an spn or find duplicate spns.

So I came up with a handful of [functions](https://github.com/mod-posh/SpnLibrary/blob/main/SpnLibrary.psm1), based on the builtin help from the setspn utility and the TechNet [article](http://technet.microsoft.com/en-us/library/cc731241(WS.10).aspx) about setspn.

Reset-Spn -HostName

#### THIS WILL RESET THE SPN FOR THE GIVEN HOSTNAME

Asd-Spn -Service -Name -HostName -NoDupes

#### THIS WILL ADD AN SPN TO A GIVEN HOST AND OPTIONALLY CHECK FOR DUPLICATES WITHIN THE DOMAIN FIRST

Remove-Spn -Service -Name -HostName

#### THIS REMOVES AN SPN FROM A GIVEN HOST

Get-Spn -HostName

#### THIS WILL RETURN THE SPN’S FOR A GIVEN HOST

Find-Spn -Service -Name

#### THIS WILL FIND ALL SPN’S OF A GIVEN SERVICE, OR OF  GIVEN NAME, OR BOTH

Find-DuplicateSpn -ForestWide

#### THIS WILL FIND ALL DUPLICATE SPNS WITHIN THE DOMAIN OR OPTIONALLY THE ENTIRE FOREST

Currently my functions are just wrappers for setspn.exe but I’m planning a V2 that will leverage .NET to handle this. I don’t get a lot of flexibility in error handling and output when I use a stand alone command.

* I want to return objects
* I want to be able to not have dependencies
* I want the flexibility of .NET
