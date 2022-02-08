---
layout: post
title:  "Working with computer objects"
date:   2010-09-21 19:26:00 -0600
categories: blog
tags: September 2010 KU SOECS Scripting
---
So I’m working with computer objects, if you can’t tell from my previous [post]({% post_url 2010-09-20-active-directory-naming-strategy %}). There are times when what you really want to know about a given computer is, who’s responsible for it. With good user education, your OU admins or computer admins will pre-stage a computer and populate the ManagedBy property. This effectively lets anyone who can read that property, know who is responsible for this object.

In an environment with thousands of computers, or even hundreds of computer’s the likelihood of this actually happening is most likely very slim. I freely admit that when I create a computer object, I leave that field empty. But it’s ok, if you have given your users the right to join their own computers to the domain this information is stored in the ACL for the object.

I got to write a new function that pulled the owner from the ACL. I found what I was looking for in the [Technet Forums](http://social.technet.microsoft.com/Forums/en-us/ITCG/thread/59159984-729c-46d1-8faa-58c71ac3a209). The only thing I changed was making the stand-alone script a function that I could pull into my code. The output is the username formatted NT-style:

> DOMAINUsername

I wrote a couple of scripts today around the managedby property. The first [script](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/GetOwner.vbs) walks through your ActiveDirectory and reports out the name of the computer and who the owner is. If you have a domain admin account and created that object then the owner becomes Domain Admins. If a non Domain Admin created the object, that account is listed as the owner.

The best way to run this particular script is as follows:

``` dos
cscript //nologo GetOwner.vbs > ComputerOwners.csv
```

The resultant output file can be opened up in the spreadsheet program of your choice, and will have two columns. The name of the computer, and the DomainUsername of the owner. You will want to change the LDAP URI at the top, to point at the OU of your choice within your domain, or for fun point it at the root. This script does not modify anything at all, and can be run as a regular non-admin user.

That was the first part of what I needed, a way to get the user who owned the computer. For the next [script](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/SetManagedby.vbs), I needed was a way to write that information into the managedby property of the object. The only issue I had was that the username was reported NT4 style, and I needed to store it as a DN. So I got to write a second function that converted the NT4 username to a DN string.

I found the code on the [Scripting Guys blog](http://blogs.technet.com/b/heyscriptingguy/archive/2007/08/22/how-can-i-get-the-guid-for-a-user-account-if-all-i-have-is-the-user-s-logon-name-and-domain.aspx). The actual code from the blog returns a GUID, but they kindly added a table at the bottom that listed the other formats that could be returned from IADsNameTranslate. I modified the code to return ADS_NAME_TYPE_1779, and changed it from a stand-alone script to a function that returned the DN of the user.

There is no output in this script, it just runs and sets as it goes. If you have a problem with that, one thing you can do is change the line that reads:

``` visualbasic
Call WriteData(strADSIProp, strUserDN, strComputerPath)
```

To

``` visualbasic
Wscript.Echo "Changing " & strADSIProp & " property of " & strComputerPath & " to " & struserDN
```

This will output a line that will tell you what the script will do. Personally I would comment out the call to WriteData and add the Wscript.Echo as a new line below that. Once you are satisfied that it works the way you anticipate, delete that echo line and uncommenet the call.
