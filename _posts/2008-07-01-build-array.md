---
layout: post
title:  "Build Array"
date:   2008-07-01 11:11:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
---
The [BuildArray](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/procedures/BuildArray.txt) procedure connects to the provided domain and returns a list of requested objects. Then it loops through those objects checking for a specific value in the DistinguishedName property. The lab structure in our domain uses the room number of a given lab as the name of the OU.

I use the InStr function to locate the room number and then based on that information the UpdateArray procedure creates the tallies in the arrLabs array.

This procedure relies on a particular Active Directory OU structure to work properly so it may require some adjustment.

This procedure accepts two parameters:

strLDAP
strObject
strLDAP
This is the LDAP URL of the Active Directory domain you wish to connect to.

strObject
This is the Active Directory Object you wish the query to return.
