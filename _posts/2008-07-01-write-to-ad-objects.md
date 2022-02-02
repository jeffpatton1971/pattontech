---
layout: post
title:  "Write to AD Objects"
date:   2008-07-01 11:10:00 -0600
categories: blog
tags: July 2008 KU SOECS Scripting
---
The [WriteData](https://github.com/jeffpatton1971/mod-posh/blob/master/vbs/playground/procedures/WriteData.txt) procedure originally was used to update the Description property of computer objects in the Active Directory. The procedure can now update any single-valued property of a given object in the Active Directory.

The procedure accepts the following parameters:

strProperty
strValue
strADSPath
strProperty
This is a string variable that would contain any one of the many single-value properties available in Active Directory. I typically pass in “Description” as that’s the property I’m interested in.

strValue
This is a string variable that holds the data to be written to the Active Directory. The information can be statically passed in or created based on calls to other functions within the script.

strADSPath
This is a string variable that holds the DistinguishedName property of the object we are working with. This is typically passed in from another function that queries the Active Directory for object data.
