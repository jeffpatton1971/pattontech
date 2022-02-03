---
layout: post
title:  "Finding Enabled User Accounts"
date:   2010-02-16 19:03:00 -0600
categories: blog
tags: February 2010 KU SOECS Scripting
---
I was recently notified that one of the computers that we’re responsible for had been compromised by [Torpig](http://www.sophos.com/security/analyses/viruses-and-spyware/trojtorpiga.html). The way I understand it is the user launches a website that might be a part of a phishing scam, and the software gets dropped on their computer. Once installed it searches through the computer and transmits usernames, passwords, social security numbers, account numbers and the like up to a group of servers on the internet.

The security group on campus, [ITSO](http://www.security.ku.edu/), discovered that on a handful of Windows XP machines that were compromised with Torpig, that the HelpAssistant account was enabled. It seems that on an XP machine where the user has admin privledges that account gets enabled and is how Torpig works it’s magic.

Using that bit of magic I created the following script, [EnabledAccounts.vbs](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/EnabledAccounts.vbs), which is basically an AD searcher script. It works through Active Directory looking for computers to connect to. Once it finds one, it gets a list of user accounts, from that list it looks for a specific account, HelpAssistant, in this instance, once found it checks to see if it’s enabled.

If the account is found and the account is enabled it outputs a simple line:

HelpAssistant account enabled on: Desktop-PC1

By no means would I consider this the answer to tracking down Torpig on your network, but it worked for us. Additionally you can use this script to determine if any account is enabled that should be disabled.

Good luck and enjoy!
