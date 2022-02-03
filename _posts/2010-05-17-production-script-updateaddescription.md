---
layout: post
title:  "Production Script: UpdateADDescription"
date:   2010-05-17 19:11:00 -0600
categories: blog
tags: May 2010 KU SOECS Scripting
---
The [UpdateADDescription](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/production/UpdateADDescription.vbs) is run hourly on a computer connected to the domain. It’s sole purpose is to loop through all the computers in a given OU and update their Description property. It serves as both inventory and somewhat of a user tracking system as it will give us an idea of who is logged on to what computer.

Most of the functions and procedures in this script have already been documented, so to get the details on how everything interacts you might look to those articles first.

I wanted to be able to use the Active Directory to hold information that was pertinent to how we deliver service to our users. I needed something that would let me know who was logged on, where they were logged and some inventory-ish data about the computer itself.

The beauty part about this is the output of the script is stored in a comma-separated format in the Description property, so in the Active Directory Users and Computers console you can literally export the view as a comma-separated file, and open it directly in any spreadsheet program.

The main fields from the view are comma-separated and the Description property, comma-separated is parsed out like additional fields.

In order to use this script in your environment I highly suggest you run it in a test environment.
