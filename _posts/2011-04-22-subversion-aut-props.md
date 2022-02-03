---
layout: post
title:  "Subversion auto-props"
date:   2011-04-22 13:06:00 -0600
categories: blog
tags: April 2011 Scripting
---
I’ve been really busy working with and learning PowerShell and dutifully committing my code into my subversion repo. For the most part it’s been really fun and I’ve not had too many problems. What I wasn’t expecting was not being able to view my scripts on my Trac site. I had recently uploaded a script I wrote that would delete local user accounts from computers in the domain. I had decided to link the help page I created to the source page, except when I browsed the repository and clicked on the file I received the following message:

> Property svn:mime-type set to application/octet-stream

Trac let me know that while I couldn’t view it, I was perfectly welcome to download it. My first thought was great, now what do I do. After a little bit of searching the above message, adding in keywords like Trac, Apache and TortoiseSVN I came across a few pages of interest. The first page was the [Subversion FAQ](http://subversion.apache.org/faq.html), about halfway down the page was the [auto-props](http://subversion.apache.org/faq.html#auto-props) heading. Now that I knew what I was looking for a change in the search brought me to [TortoiseSVN project settings](http://tortoisesvn.net/docs/release/TortoiseSVN_en/tsvn-dug-propertypage.html) page.  Following the document I found the Subversion properties for my scripts folder, and then defined a mime-type for the PowerShell and VBScripts files. Since there is nothing particularly special about these files, I set their mime-type to text/plain.

After committing my change (everything is version controlled in subversion), I went back to the browser, and was greeted with the same message. Even though I checked the box to apply the property recursively, this setting only applies to new files and not existing ones. So I solved the problem moving forward, but I still needed my scripts to open, I browsed to the file in my local repo and went into it’s properties and noticed that it’s mime-type was set to application/octet-stream, sound familiar?

After removing that property the links work, so I decided to write this up really quick, because I’m sure I’ll hit it again. If you find it useful as well, awesome!
