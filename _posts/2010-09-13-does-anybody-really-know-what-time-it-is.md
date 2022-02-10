---
layout: post
title:  "Does anybody really know what time it is?"
date:   2010-09-13 19:24:00 -0600
categories: blog
tags: September 2010 KU SOECS
comments: false
---
Or, How to fix your Novell clients so they don’t get hosed up times!
If you don’t know this, when a computer in an Active Directory domain gets an invalid time it stops working. It has to do with Kerberos tickets and time stamps, and really it’s all very boring. Suffice it to say, that is the reason there is a w32time service. By default, Windows clients in a domain sync their clocks to the domain controllers.

But something interesting happens when you add the Novell client into the mix. I want to state for the record, I have almost no experience with Novell, nor do I care for the product. In my experience, once the Novell client is added it appears to over-ride the default behaviour of Windows and force a time-sync with it’s own server. Aside from the obvious problems one would have with that particular arrangement, the next thing that seems to happen is, for no apparent reason the client will get the wrong time. Check out the image below of one very sad and confused computer.

![20100913-time](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100913-time.png)

Like I said earlier, I claim no ability to admin a Novell server at all, it’s just not my thing. One thing I have been assured of is that the time on the Novell server is correct, and I have no reason to believe otherwise. The internet says that we should load up the latest service pack, sadly we’re already there. The internet also says we should change the default settings of the Novell client.

``` dos
HKLMSOFTWARENovellNWGINALogin ScreenSynchronize Time = 0 (REG_DWORD)
```

We just rolled that out, I am fairly certain that will take so I don’t think I’ll worry about this again. To be quite honest the only reason I’m even posting is because I really enjoy the image above, and I hope you do as well!
