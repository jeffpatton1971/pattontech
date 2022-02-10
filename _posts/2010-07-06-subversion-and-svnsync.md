---
layout: post
title:  "Subversion and svnsync"
date:   2010-07-06 19:18:00 -0600
categories: blog
tags: July 2010 Personal
comments: false
---
First things first, I am in no way proficient in how subversion works, but I am usually pretty good at reading documentation and then being able to work out issues I encounter.

After reading [Chapter 5](http://svnbook.red-bean.com/en/1.5/svn-book.html#svn.reposadmin) of the [svn book](http://svnbook.red-bean.com/en/1.5/svn-book.html) I decided to try my hand at svnsync. It seemed a nice way to make a portion of your code publicly available, like say tagged releases. I followed the book pretty much, and on my dev server was able to get it go. I was not as lucky on my regular server.

The first issue I had was comprehension, even though everywhere you look the syntax shows:

``` dos
svnsync init DESTINATION SOURCE
```

I kept reading that backwards, so if it helps you may want to try setting those as [environment variables](http://journal.paul.querna.org/articles/2006/09/14/using-svnsync/). I saw that tip on a site as I was reading about problems I was having.

The second issue that I ran into was how authentication was setup, which was really more of an apache config issue. Once I straightened that out, I was able to authenticate but was unable to perform the initialization. This one had me stuck for a little while, until I read this [post](http://www.svnforum.org/2017/viewtopic.php?t=8425).

I limit what can be done to valid-users in my conf, so adding PROPPATCH to the list of things to allow valid-users to perform, allowed the initialization to happen.

After the initialization comes the commit, I kept getting a message that the start-commit hook was blocking the transaction. So I simply commented out everything in the start-commit script minus the first line and the sync went through just fine.

The next thing I needed to make happen was the newly sync’d repo to be publicly available. Pointing my browser at the destination url gave me a login prompt, so it was back to my apache conf file. Removing the GET property from my LIMIT list made the repo readable without a login.

The last thing was to make sure that the repo could be downloaded but prevent commits by anonymous users. What appears to work is modifying the LIMIT list to the following properties:

This allows me to checkout from my new sync’d repo without logging in, but prevents me from making a change to the repo via commit. This may not be the best way, or even the preferred way but it appears to work pretty well.
