---
layout: post
title:  "Showing off some DSC Resources"
date:   2013-12-31 11:07:00 -0600
categories: blog
tags: December 2013 KU IT Scripting
---
Yesterday I wrote three articles ( [Part 1]({% post_url 2013-12-30-dsc-part %}), [Part 2]({% post_url 2013-12-30-dsc-part2 %}), [Part 3]({% post_url 2013-12-30-dsc-part3 %}) ) about [Desired State Configuration](http://technet.microsoft.com/en-us/library/dn249912.aspx). I thought I would post a slightly more complex Configuration. This configuration performs several actions on the target node.

1. Install the Web-Server feature
2. Install several additional features that depend on the Web-Server
3. Install the [WebDeploy application](http://www.iis.net/downloads/microsoft/web-deploy)
4. Configure Windows Firewall to allow WebDeploy traffic

This particular Configuration has some nice features to note. The first of which are the parameters. You must provide a ComputerName (or guid) and a path to the WebDeploy MSI. You can also optionally specify a [source path](http://technet.microsoft.com/en-us/library/jj127275.aspx) for the features, this is useful if you have cloned a server.

You will also notice that nearly all the Resources use the DependsOn property. Since all the features are web server related, I set the DependsOn property to be the WebServerRole. If you look in the documentation I believe that Microsoft has this down as Requires, but I believe it’s changed since the docs came out.

The [Package Resource](http://technet.microsoft.com/en-us/library/dn282132.aspx) installs WebDeploy. The ProductID I was able to pull from the MSI using [ORCA ( SDK Download )](http://www.microsoft.com/click/services/Redirect2.ashx?CR_EAC=300135395). If you don’t have that installed, or don’t want to install it, you can install WebDeploy on a reference machine and ought to be able to query the [ProductID from WMI](http://technet.microsoft.com/en-us/library/dd347651.aspx).

The Script Resource was a little more difficult for me, and thankfully I found a wonderful [article](http://blog.cosmoskey.com/2013/10/) that did the deep diving. Basically a Script has three scripts that need to run. The TestScript must evaluate to True or False. If the TestScript == False then the SetScript runs. The GetScript must return a HashTable, and the only thing that it needs to return is the Result property, but you can also specify the contents of the GetScript, TestScript and SetScript scripts. Finally the SetScript is the script that will do the thing you need done. In this example create a firewall rule to allow port 8172.

So basically what happens is when you run Start-DSCConfiguration, the script will perform a test. If that test returns true then we can assume that the thing we need done is done. If that test returns false then we need to do the thing, whatever that thing is.

When you run Get-DSCConfiguration, the script will get the state of what we did, which is why all we need is a result.
