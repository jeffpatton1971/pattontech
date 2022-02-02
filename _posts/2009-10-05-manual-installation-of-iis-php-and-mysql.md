---
layout: post
title:  "Manual installation of IIS, PHP and MySql"
date:   2009-10-05 14:16:00 -0600
categories: blog
tags: October 2009 KU SOECS
---
Last week I posted an article about how I configured IIS, PHP and MySQL using Microsoft’s Web PI 2.0 technology, which was wicked easy. Today I figured I would run the same thing but this time “manually”. Which if I had to rank things in order of difficulty from 1 to 5, where one is the easiest, the Web PI install would be a one, and the manual install would be a two. There are a few things you will need to download the links are below.

* [FastCGI](http://www.iis.net/extensions/FastCGI)
* [PHP](http://windows.php.net/download/)
* [MySQL](http://dev.mysql.com/get/Downloads/)

Windows

Setup your server in whatever way suits you best, make sure that you install the Application Server role, I chose to not check the ASP.NET and Frontpage extensions, but that’s my personal preference. Depending on how you setup your server you may or may not be asked to insert the media.

FastCGI and PHP

You should download and install FastCGI first, just take all the defaults. Once you have that configured you will need to download and install PHP, I’ve been sticking with version 5.2 but you can grab the most recent if you like. PHP will configure FastCGI for you if you install it first. If you do the FastCGI after the fact you may have some configuration you will need to do manually.

MySQL

Again just take the defaults depending on which version fo MySQL you downloaded, I did not choose the Windows Essentials build, I chose the larger installer option. During setup I selected to install the complete package. Once the installation is complete you will want to choose the Configure option as this allows you to setup your root account on the server.

I used the Standard Configuration option, chose to install it as a service and launch automatically. The following screen is where you define the password for root, set this as secure as you need it. I did not select to make the account available from remote machines, you may want to note this password unless you have a standard admin password that you use as future configurations will require it.

Click execute and you’re done.

At this point everything is done the next thing we’ll need to do is tackle any error messages that show up. I have already documented one regarding [msvcrt7.dll](2009-10-04-php-pear-and-windows.md), I have noticed that on my test machine I have other messages which I will document later tonight or tomorrow.
