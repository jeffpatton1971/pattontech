---
layout: post
title:  "Configure Quickbooks for Windows Server"
date:   2008-10-09 11:17:00 -0600
categories: blog
tags: October 2008 KU SOECS HOWTO
comments: false
---
In a client/server environment multiple users connect to the Quickbooks server to access data files. There are two methods for this:

1. Configure a file share on the server and allow the clients access to this share. In this scenario the client machines themselves will “host” the data files. Whichever client was “hosting” the files last will be saved into the data files. The problem with this arrangement is if the previous “hosting” client isn’t available you will receive several network connectivity errors.
2. Configure a file share on the server and allow the clients access share. In this scenario the client machines access the data files through the Quickbooks Database Manager. The server will authenticate the users as opposed to a given client workstation. The problem with this arrangement is apparently Quickbooks is unable to make their database manager a service which means a user must be logged on so the application runs, so the following procedure should be followed:

Quickbooks Database Manager Service

1. Create a service account
2. Place it into the local administrators group
3. Create a scheduled task that runs the Quickbooks Database Manager on computer startup
4. Configure the task to run in the context of your server account
