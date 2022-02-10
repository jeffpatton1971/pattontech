---
layout: post
title:  "Google Reader feed to Blogengine blogroll.xml"
date:   2011-02-18 13:03:00 -0600
categories: blog
tags: February 2011 Personal
comments: false
---
So I wanted the list of blogs I follow in Google Reader to be displayed on my website. [Blogengine](http://blogengine.codeplex.com/) uses XML files for just about everything, so I decided I would see how difficult it was to convert the Google Reader OPML to a format suitable for Blogengine. The first step is to export your list to an OPML file, once you have that you need to grab this XSLT file that will handle the conversion and finally some sort of utility that will read in the OPML and XSLT and output the appropriate XML.

Getting your feed

Login to Google Reader, and navigate to Manage Subscriptions. You can find that link in the lower left of your reader display. From there you will need you need to click the Import/Export tab, and then just click the “Export your subscriptions as an OPML file” link.

![image](https://prdwebappstorage.blob.core.windows.net/pattontech/images/image.png)

That file will download to your computer, you’ll want to remember where it goes as you’ll need it shortly. Once you have that file you will need the following XSLT file. I found this code on [codeplex](http://blogengine.codeplex.com/discussions/62162) where the original is located.

You should save that as a file with a .XSLT extension, ideally this would live in the same folder as the .OPML file you received from Google. Now you need something that will convert those two files into what you need, blogroll.xml. I found the utility on Microsoft.com, [msxml.exe](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=2fb55371-c94e-4373-b0e9-db4816552e41&displaylang=en) and it did the trick, you will just need the msxml.exe file. I downloaded that to the same place as my OPML and XSLT files.

Now we can create our file, the syntax is pretty straightforward, and actually quite flexible you can literally key in your XML and XSLT from stdin. But this is what my syntax looked like.

``` dos
msxsl.exe google-reader-subscriptions.xml google-reader-to-blogengin.xslt -o blogroll.xml
```

The first parameter is your input XML file, the second parameter is the XSLT file that will be used to create your output (-o) file. The resulting blogroll.xml file can be copied into your APP_DATA folder on your Blogengine installation, and you may or may not need to restart the webserver before it shows up. If you don’t have the Blogroll extension displayed you’ll need to login and add that to the site.
