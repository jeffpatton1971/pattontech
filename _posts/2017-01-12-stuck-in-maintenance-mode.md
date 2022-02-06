---
layout: post
title:  "Stuck in Maintenance Mode"
date:   2017-01-12 15:00:00 -0600
categories: blog
tags: January 2017 Personal
---
It’s been ages, I do have plans to start writing again, and in fact this particular posting came about because I was going to write up something I just came across at work. I went to login to my site, and was presented with a 500 error. Since I’ve recently moved the entirety of both my sites to Azure Web Apps, I was mildly concerned as I had no real access to any of the switches, buttons or levers that make things go. I checked to see if I could login to my other site and received the same error message, which I guess is good. They are both running on the same App Service in Azure and while the actual pages would load and such I just was unable to login.

I thought about restarting the App Service itself, but chose to start small. I restarted the Web Application and was able to login, I then repeated that process on the other site and logged in to it as well.

That’s when I noticed that there were some updates available, specifically the plugin for Windows Azure Storage, and WordPress itself. So I updated WP first, it took longer than HTTP was willing to hang around so I got a nifty timeout. Reloaded the page and it came back, and I updated the rest of my plugins. Then I rolled over to update this site, same process, same plugins, same timeout. Only a refresh didn’t bring the site back after the plugins updated. I gave it a little more time and still I got the maintenance page. Keep in mind there is no server that I have access to, to really do anything that I would normally do. So i did a search and came across this article.

[https://premium.wpmudev.org/blog/wordpress-stuck-in-maintenance-mode](https://premium.wpmudev.org/blog/wordpress-stuck-in-maintenance-mode)

Apparently the update process drops a hidden file .maintenance in the root of the site, neat. All I needed to do was delete that file, but how do you do that when you don’t have a thing to login into?

Turns out Web Apps have a console, this can be found under the Development Tools on the Web App. This drops you into a shell on the root of your site, even though I know this is Windows it felt like Linux, so

``` bash
uname -a
D:\home\site\wwwroot
MSYS_NT-6.2-WOW RD0003FF42E552 2.5.0(0.295/5/3) 2016-03-31 18:26 i686 Msys
```

That’s neat, all I needed to do then was run ls -a to make sure .maintenance was there, and then just rm .maintenance to get rid of it, refresh and the site is back up and available. I’m going to paste the contents of that page here, in case it goes away.

Has your WordPress site ever gotten stuck in maintenance mode? Many people seem to run into this problem. They update a WordPress theme or plugin or maybe even WordPress itself, and in the middle of the update, something goes wrong, and their site gets stuck in maintenance mode with nothing appearing but the message, “Briefly unavailable for scheduled maintenance. Check back in a minute.”
Not only are visitors locked out, but even the Admin is locked out of the backend.
If this happens to you, try one of the following solutions.

1. The solution that seems to work for most is to delete a file called “.maintenance” from the root of your site. This is a temporary file that gets created in the update process, and more than likely, this is your culprit. (Notice the *dot* at the beginning of the file name.)
Again, this file rests on your server in your main WordPress install section. You CANNOT access it through the Admin area of your site. You will need to access the server through your webhost’s system (like Cpanel) or via FTP.
If you do not have access to files on your server, then contact your host and let them know the problem (and the solution, of course).
2. Although removing the .maintenance file seems to work for most, it doesn’t work for all. If it doesn’t work for you, then try the following:
   1. Delete the .maintenance file as outlined in option #1 above.
   2. Delete the plugin or theme that you were attempting to update.
   3. If your site does is not back at this point, then in your wp-content folder, you will find a folder called “upgrade.” Delete the files or folders you find there.

> Remember to clear the cache in your browser (or use a different browser) to make sure that you aren’t getting an old version of your site.
Once you have your site back, you may want to run your updates again to make sure they’ve taken.
