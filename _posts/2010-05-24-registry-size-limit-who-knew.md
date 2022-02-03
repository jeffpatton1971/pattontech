---
layout: post
title:  "Registry size limit...who knew?"
date:   2010-05-24 19:14:00 -0600
categories: blog
tags: May 2010 KU SOECS
---
This past Friday we pushed out 30 new lab machines. These machines were imaged the same way we image the rest of our labs. We noticed that several applications failed to install, and Carson began troubleshooting that. Turns out, due to the number of installed applications we ran out of space in the Windows Registry. Call us noobs if you like, but really…there is a max?

We found a few articles on the subject and looks like this was a setting that started for Windows NT. Here are a few of the articles we found:

* [Technet: Windows NT Regsitry](http://technet.microsoft.com/en-us/library/cc751456.aspx)
* [Technet: RegistrySizeLimit](http://technet.microsoft.com/en-us/library/cc963194.aspx)
* [Microsoft Answers](http://social.answers.microsoft.com/Forums/en/w7programs/thread/321fe20a-ecc6-469e-93a4-b640f39c05b5)

We set ours to 0xffffffff which places it at approximately 4gb. The way I understand it is if this value is over 80% of the amount of paged pool, then Windows set’s it at the 80%. Unset, I believe Windows will automatically adjust this for you, but I’m not really sure on that. I rolled this fix out as a GPO Preference to only lab computers.

The issue presented itself in a few ways, first Windows Updates was broken, additionally multiple .msi installations failed with cryptic error codes.
