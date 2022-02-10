---
layout: post
title:  "Evolution and Exchange 2010"
date:   2010-12-22 19:35:00 -0600
categories: blog
tags: December 2010 KU SOECS
comments: false
---
So you want [Evolution](http://projects.gnome.org/evolution/) to attach to Exchange natively, not unlike Outlook? The simple answer to your question is [evolution-mapi](http://www.go-evolution.org/MAPIProvider). This lovely plugin allows you to connect over [MAPI](http://en.wikipedia.org/wiki/Messaging_Application_Programming_Interface), to an Exchange server. I believe the supported servers start with Exchange 2003, and certainly go up to Exchange 2010 since that’s the version of Exchange we use on campus. The setup is fairly straightforward, but I have included screenshots at each step.

First things first, I’m running Ubuntu 10.10 on my laptop, the mapi plugin is not installed by default with Evolution, so you will need to get that first.

``` bash
sudo apt-get install evolution-mapi
```

Personally, I tend to run an update and upgrade prior just to make sure I’m all caught up. Once it’s installed all you need to do is launch Evolution. If this is the first time you’ve started Evolution you will see the following screen.

![20101222-evolution-1](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101222-evolution-1.png)

Click forward.

![20101222-evolution-2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101222-evolution-2.png)

If you already have an evolution config backed up, you can restore at this screen, otherwise click Forward.

![20101222-evolution-3](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101222-evolution-3.png)

Fill in your personal information, click Forward.

![20101222-evolution-4](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101222-evolution-4.png)

From the Server Type drop-down you will want to select Exchange MAPI, this will change the list of options available to fill out.

![20101222-evolution-4a](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101222-evolution-4a.png)

The name of your exchange server, your username and the FQDN of your domain get filled in here. Click Authenticate to be prompted for your password, and a test connection to be made. Click Forward when you have everything filled out properly.

![20101222-evolution-5](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101222-evolution-5.png)

Some preferences regarding frequency of checking for mail, and what to do with mail handling, click Forward when you’re ready. You can change these after you get into the client.

![20101222-evolution-6](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101222-evolution-6.png)

A descriptive name for your account if you have multiple email accounts that you check with Evolution, click Forward.

![20101222-evolution-7](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20101222-evolution-7.png)

You’re all done, once you click Apply, your preferences will be saved and once the mailbox sync’s you will have all of your mail available to you.
