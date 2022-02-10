---
layout: post
title:  "Bulk URL Monitoring"
date:   2015-06-05 13:00:00 -0600
categories: blog
tags: June 2015 KU IT
comments: false
---
How did I not know about this before? So I’m working on creating a Management Pack for Advanced Group Policy Management, and hunting for the utility to seal a MP. I find this utility buried in the Tools folder. I’m going to link the article I found on using it here as well as scrape the text in case it goes away.

[Original Article](http://www.scom2k7.com/bulk-url-editor/)

Bulk URL Manager

The Bulk URL Editor was introduced in SCOM 2007 R2.  I don’t often see this tool used as most customers don’t even know it exists or don’t understand the benefits of it.  The first benefit of the Bulk URL editor is that it scales to thousand of URLs.  If you were to try to create hundreds of URLs with the Web Application Templates it won’t work.  I have tried this in the past and there are so many workflows running at the same time that the agent fails and you end of not monitoring anything.   The second benefit of the tool is that you can add a bunch of websites in a few minutes.

The Bulk URL Editor is not very intuitive, but once you understand how to use it the process is pretty easy.  If you haven’t used the tool I highly recommend giving it a try.

TechNet has some good documentation [here](http://technet.microsoft.com/en-us/library/dd788987.aspx).

To use the Bulk URL Editor I copy the tool from the installation media.  The file is stored in the “SupportTools\AMD64” directory

On my computer that has the SCOM console installed, I copy the “BulkUrlManager.exe” file to “C:\Program Files\System Center 2012\Operations Manager\Console” (If you copy it anywhere else it won’t work)

I double click on the “BulkUrlManager.exe” file.

On the Connect to Server dialog box I type in the name of my Management Server and click connect

I click the New Icon

I then type in a name of my website template.  I choose “Standard URL Monitoring”

Now I click Create a new Management Pack,

I then give the management pack the name. I choose “BUE Website Monitoring” and click OK

I click OK on the Add New Template Screen

On the next dialog box I click Yes, then OK

Under Templates I click the template I created called “Standard URL Monitoring”

Now I click Add

Now I simply add the URLs that I want to monitor (Note: You need to add http:// or https:// or it will fail)

I click OK and I see all of my URLs are attached to my Standard URL Monitoring Template

Now I simply hit save. I click yes to save the changes to the selected web template.

I am done in the Bulk URL Editor for now. But I am not finished setting up my URL monitoring.  I need to select where I want the URLs to be monitored from.

I launch the Operations Manager Console and go to the Authoring screen.

I expand out Web Application and right click to refresh the screen.

Now I see the website I created using the Bulk URL editor

Under the Actions pane, Custom Actions I click Edit web application settings

My website opens and all I see is a string of text that looks like a ugly variable.  (Don’t panic this is how it is supposed to look)

Now under the Actions Pane, under Web Application I click Configure settings

I click the Watcher Node tab and select the server I want to run the website monitoring.  I choose my Management Server.

I click OK and then click Apply at the bottom of the Web Application Editor screen

I then close the screen with the red X at the top right

Now I go back into the Bulk URL Editor.

I select the Template I have been working with and hit Synchronize (you may need to refresh before the Synchronize button lights up)

I click Yes,

I close the Bulk URL Editor as I am done with it.

Now I open the SCOM Monitoring Console and look for our Web Application, Standard URL Monitoring Instances.

I can see all my websites are now being monitored.

As you can see each website is its own object.  This I nice for putting them into maintenance mode putting them into groups.

I go to groups I can see that the Bulk URL editor also created a group.
