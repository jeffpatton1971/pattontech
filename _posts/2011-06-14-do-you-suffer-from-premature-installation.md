---
layout: post
title:  "Do you suffer from 'Premature Installation'?"
date:   2011-06-14 13:09:00 -0600
categories: blog
tags: June 2011 KU SOECS
comments: false
---
Or, “What’s in a name?”

Turns out a whole hell of a lot! First I need to thanks [Nick](http://nicholasstudt.com/) for the awesome title, as he completely pinpointed my issue after I told him what happened! The last article I posted talked about our desire to move away from vanilla Windows 2008 and up to Windows 2008 R2. What should have been a pretty straightforward process got slightly mangled by two things. I forgot to rename the computer, and I moved to fast, hence the “Premature Installation!”

Naming is important, there are some names you can change and some you can’t. How computers get names has also changed with 2008, it used to be that during installation you were prompted for a name, now you do that after. One of the things we found out was that a Domain Controller can have [multiple names](http://technet.microsoft.com/en-us/library/cc816601(v=ws.10).aspx), while I don’t know how recent that change is, or isn’t it was new to us. Back to the naming process, while there’s nothing inherently wrong with a Domain Controller named WIN-LLF3467Q0, you would undoubtedly agree it doesn’t really roll off the tongue.

So that was the first problem, I installed Windows 2008 R2 without mishap, and Directory Services installed, and when I hopped over to the Domain Controller’s OU I noticed my problem. So the first thing I did was go to the above article and renamed my new Domain Controller, and this is where the second problem occurred.

Replication while speedy, it does take time, and the more things you have in your AD the longer it could potentially take. The end result of my fubar is that we wound up with no less than three different entries in DNS for the same server, only one of which was correct, and due to replication latency the name of the server in AD was completely wrong.

So I did what I imagine most people would do, and went to uninstall DS from the server and attempt to start over. But because things had gotten so trashed I was unable to uninstall DS, because the server name that I was on didn’t exist in AD, I really should have screenshot stuff but take my word, I was on dc1 and the error was dc1 didn’t exist…which was technically true. It was a crazy weird edge situation, you could actually connect to DC1 but you had to type it in manually in order to get there. At any rate I was unable to remove DS, so I turned off the computer and attempted to remove the computer account that was listed from the Domain.

The problem with that was in order to do it, you MUST be on a Domain Controller to remove a non-functional Domain Controller from the Domain. I’ve not found an article on TechNet that mentions that, but I’ve not looked in any great detail. This information was found on the [TechNet Social](http://social.technet.microsoft.com/Forums/en/winserverDS/thread/554a9207-42fd-4b45-bb80-63eb601fd7fb) site, after connective over RDP to the off-site Domain Controller I was able to remove the offending account.

So, in the future, remember to be patient and make sure you have a checklist!

1. Install Windows OS
2. Change the default name before network connectivity
3. Make any needed changes
   1. [Disable IPv6](http://support.microsoft.com/kb/929852)
   2. [Apply 3rd party DNS Hotfix](http://support.microsoft.com/kb/977158)
4. Install Directory Services
5. Wait
6. Wait
7. Wait
8. Verify successful replication

These are the steps I followed on my server rebuild yesterday, as well as the same instructions I followed when I migrated the second Domain Controller this morning.
