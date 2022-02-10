---
layout: post
title:  "Delegation of OU and GPO's"
date:   2010-08-17 19:20:00 -0600
categories: blog
tags: August 2010 KU SOECS
comments: false
---
Why does printing need to be so complicated? Admittedly a portion of the problem falls on us running our own domain. At times it can be irritating, like now, and at times it can be a lifesaver. So this is our situation, we run an independent ActiveDirectory domain that has an external trust with the main campus domain. Our domain is a resource domain in the NT4.0 version of the term. We have almost no actual user accounts in our domain, but all of the computers we manage exist in our domain.

The University recently rolled out a new Multi-Function Print Device contract. Central IT will maintain control over the printers and we’re responsible for the consumables. Printing has been [problematic] for us, but we managed to get a good working solution going. With these new printers, we decided to leave them on Central’s print server and them push them via Group Policy.

At first blush that appeared to work perfectly fine. All of the computers in our office were able to connect and see the printer, the one thing worth noting is that all of our computers are Windows 7. When I fired up my Windows XP computer, the printer showed up, but nothing else. Attempting to print, access properties, or open the queue resulted in an error.

We started working on this problem, originally thinking maybe it was a cross-forest trust issue. That was quickly tossed as the Windows 7 computers could connect to the printer, and people who had the printer manually added could print. Adding the user to the administrators group sort of worked, but as we don’t really roll that direction with our users we decided against that route as well. We started working with tweaking group policies for printing, defining approved servers in both Package Point and print (doc), as well as [Point and Print Restrictions](http://support.microsoft.com/kb/319939).

It turns out that we were going about it backwards, the Point and Print Restrictions policy was the correct policy for Windows XP. But instead of enabling the policy and defining a list of print servers, the key was to DISABLE the policy. Part way down the above KB article is the following bullet:

> “If you set the policy to Disabled, users can use the Point and Print functionality to select any shared printer they have access to.”

For clarification there was a thread from 2004 on [tomshardware.com](http://www.tomshardware.com/forum/120039-45-policy-effect-computer), where a Microsoft tech added the following:

> “When the policy setting is enabled, the client can be restricted to only point and print to a server within its own forest, and/or to a list of explicitly trusted servers.
>
> When the policy setting is not-configured, it defaults to allowing point and print only within the client’s forest.
>
> When the policy setting is disabled, client machines can point and print to any server.”

To test we created a policy with nothing in it, aside from disabling Point and Print. We booted up a fresh install of Windows XP and a user with no administrative access and were able to connect to the printer and send a test page.

Success!

I created a simple script that I placed on our public share. The script’s sole purpose in life is to map a single printer, this printer is provided via the parameter box in Group Policy. It connects to a print server that you set inside the script, maps the printer share that you provide as a parameter, and then either makes it the default printer or doesn’t.

Here is the [link](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/mfd.vbs) to the source of the script.
