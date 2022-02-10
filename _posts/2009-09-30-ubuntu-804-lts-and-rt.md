---
layout: post
title:  "Ubuntu 8.04 LTS and Request Tracker"
date:   2009-09-30 14:11:00 -0600
categories: blog
tags: September 2009 KU SOECS
comments: false
---
Looking at some different possibilities regarding desktop support, so I’m outlining the steps I took to get a working copy of RT up and running on an Ubuntu 8.0 LTS server.

I’m setting this up in a VM on my desktop, since it’s Virtual PC I need to add the following at the startup and then later in the menu.lst file.

``` bash
vga=771 noreplace-paravirt
```

Install LTS like you normally would, the only option I chose at the end was the OpenSSH that way if/when I forget to configure the noreplace nonsense I can still login and work.

From what I’ve seen the only version of RT available through the repos is 3.6, which I’m sure isn’t that big of a deal, currently Best Practices is on release 3.8. I have found a couple of articles that talk about setting things up which I’ll list at the end of this posting.

The following command will show you a list of packages available:

``` bash
sudo apt-cache search rt3
```

I’m going to install the following packages:

* rt3.6-apache2
* rt3.6-clients
* request-tracker3.6
* apahce2-doc
* postfix
* lynx
* postgresql
* libdbd-pg-perl

``` bash
sudo apt-get install rt3.6-apache2 rt3.6-clients request-tracker3.6 apache2-doc postfix lynx postgresql libdbd-pg-perl
```

For postfix choose an internet site, you can go back later and configure anything else you may need.

Once everything is installed you will want to configure your RT_SiteConfig.pm file, this appears to contain just general settings for the system.

``` bash
nano /etc/request-tracker3.6/RT_SiteConfig.pm
```

Once that is done you will want to create a user account that has the ability to manipulate the database.

``` bash
sudo su
su – postgres
createuser -s -P dba
exit
```

You should now have a user who can login and do dba-type things. I was following the guide up to this point, and this is where I deviated, I created the user first then ran the script and things seemed to go fine.

``` bash
sudo su
su – postgres
/usr/sbin/rt-setup-database-3.6 –action init –dba dba –prompt-for-dba-password
exit
```

Now that you have a database you will need to configure apache so you can serve it up. I’m using the default sites-available file as we’re testing this out, so if you’re customizing things you may want to choose a different method, but either way I beleive you will need to edit the VirtualHost section and add the following line.

``` bash
Include /etc/request-tracker3.6/apache2-modperl2.conf
```

Then you will need to enable mod-rewrite

``` bash
a2enmod rewrite
```

Once that is taken care of you should be able to restart apache and browse to <http://yourserver/rt> and see things.

``` bash
/etc/init.d/apache2 restart
```

You should now see a login prompt, user is ‘root’ and password is ‘password’. You may want to check the main wiki for BestPractical to see how to configure the system once installed.

Resources:

[BestPractical wiki](http://wiki.bestpractical.com/view/UbuntuHardyInstallGuide)
