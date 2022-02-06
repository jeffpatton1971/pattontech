---
layout: post
title:  "Vmware Update Manager not responding"
date:   2011-05-19 15:19:00 -0600
categories: blog
tags: May 2012 KU SOECS
---
I received a lovely notice this morning as I was working through my servers and performing updates. I decided I would check my ESXi servers for updates using the VMware Update Manager plugin. This lovely plugin will go out and grab updates for your servers from VMware and I think optionally for other sources you define, but not today.

![vum-error](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vum-error.png)

I googled around and found some promising threads on VMware’s forums, but nothing seemed to do the trick for me. Then I found this [KB](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1011858) article, while not quite exactly what I was experiencing, it was very close. Originally I didn’t think that this article applied to my situation as my SQL instance is not using Windows Authentication, and my service runs as localsystem.

But when I looked in the vci-integrity.xml file I noted that there was a URL that was pointing at an IP address. Since IP’s are dynamic for me, I changed this to the hostname of the server, and all was right in the world!

I’m not sure why an IP address was listed in there, I assume this is done at install and most likely that IP address was the IP of my server at the time, and it recently changed so it no longer worked. Some might say that I should hard set my server IP addresses, I say your installer shouldn’t assume that an IP address will always be the same. After all how hard is it to find out if the host IP is static or dynamic?
Not hard at all anymore…
