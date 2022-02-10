---
layout: post
title:  "DSC + WinRM + GPO"
date:   2015-07-21 13:02:00 -0600
categories: blog
tags: July 2015 KU IT
comments: false
---
Ok, so I’m working on Desired State Configuration at work, and I had created a GPO to manage the WinRM settings a long while ago. This allows me to control how WinRM works and so forth and was needed for PowerShell to just work on our systems.

Fast forward to today, I’m joining some new servers to the domain, copy my configuration down, and then run Start-DscConfiguration and receive a nasty error:

The WinRM client sent a request to an HTTP server and got a response saying the requested HTTP URL was not available. This is usually returned by a HTTP server that does not support the WS-Management protocol.

That’s no good, it appears as though DSC is unhappy with WinRM, so I run through the usual set of commands

``` powershell
Enable-PsRemoting -Force
```

Still get the error

``` powershell
Disable-PsRemoting -Force |Enable-PsRemoting -Force
```

Still get the error

``` dos
WinRM quickconfig
```

Still get the error, see WinRM is configured, it turns out while it’s configured just enough for PowerShell to work, it’s not configured enough for DSC to work. I found this thread on Technet

[https://social.technet.microsoft.com/Forums/systemcenter/en-US/d3286893-3d3c-4991-a7ba-a9fd07e58288/scvmm-2008-r2-install-error-2927-0x80338113?forum=virtualmachingmgrsetup](https://social.technet.microsoft.com/Forums/systemcenter/en-US/d3286893-3d3c-4991-a7ba-a9fd07e58288/scvmm-2008-r2-install-error-2927-0x80338113?forum=virtualmachingmgrsetup)

The context is for Virtual Machine Manager but the errors are the same, it linked to this TechNet blog article

[http://blogs.technet.com/b/scvmm/archive/2011/09/23/vmm-2012-rc-understanding-the-hyper-v-host-addition-operation-if-window-remote-management-winrm-is-configured-using-group-policy-gpo-settings.aspx](http://blogs.technet.com/b/scvmm/archive/2011/09/23/vmm-2012-rc-understanding-the-hyper-v-host-addition-operation-if-window-remote-management-winrm-is-configured-using-group-policy-gpo-settings.aspx)

The good part is at the bottom under supported configurations. In my GPO I had only the https listener enabled, so I enabled the legacy listener. Additionally I did NOT have the ipv6 filter set to ‘*’ so I did that as well.

The really confusing thing for me was where to find the ipv6 setting “Allow automatic configuration of listeners” it appears that I did not have that in my GPO. Another quick search and I found this TechNet thread

[https://social.technet.microsoft.com/Forums/en-US/e4aa3b95-608f-46c3-af06-06f57b02b455/why-dont-i-have-the-allow-automatic-configuration-of-listeners-group-policy-option-for-winrm?forum=winserverGP](https://social.technet.microsoft.com/Forums/en-US/e4aa3b95-608f-46c3-af06-06f57b02b455/why-dont-i-have-the-allow-automatic-configuration-of-listeners-group-policy-option-for-winrm?forum=winserverGP)

I don’t have it, because it was renamed, ‘Allow remote server management through WinRM’. I tried to comment on the article, but I think it’s too old so I decided that I will most likely run into this again at some point.

So, here we are, another blog posting down.
