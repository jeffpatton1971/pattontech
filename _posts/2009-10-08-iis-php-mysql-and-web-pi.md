---
layout: post
title:  "IIS, PHP, MySQL and Web PI"
date:   2009-10-02 14:14:00 -0600
categories: blog
tags: October 2009 KU SOECS
comments: false
---
I’ve posted a couple of things last week regarding how I use Virtual PC, and since recently I’ve been downloading web apps that need php and mysql I figured I would document what I did to make them go. One of the things I figured out very early on is my job is to find things to make us go, yes I stole that from a [Star Trek](http://www.imdb.com/title/tt0708768/) episode but it’s so true!

So when I make new test servers I follow the steps outlined in this [post]({% post_url 2009-10-02-building-servers %}) first.

There are several ways to configure PHP and MySQL I’ll outline a couple the first would be the super easy installation which seems to get a functional install up and running quite quickly.

Web Platform Installer 2.0:

1. If you don’t already have Web PI get it [here](http://www.microsoft.com/web/downloads/platform.aspx).
2. Once installed, launch the Installer from the Start menu
3. Configure Web PI to see what we need
   1. Click “Options” select “Developer Tools”
4. Next select the following components from “Web Platform”
   1. Web Server: “IIS 6.0”, “Fast CGI” and “URLScan 3.1”
   2. Frameworks: “PHP 5.2.11”

Getting mySQL to show up is a little tricky, you will need to select a web app that depends on a database, the easy option is Acquia Drupal. This will get everything up and running as well as having a highly customizable Content Management system. Once you select Acquia Drupal click Install and review your options, you will see the MySQL Windows Essential option is selected, you could at this point click the “X” next to Drupal and be done with it. Choose accept to get the process started and depending on your connection speed you should be done within 30 minutes. You will be prompted for to configure the root password for MySQL.

Once everything is complete you may want to flesh out your PHP installation, by downloading [pear](http://pear.php.net/). I noticed that there are only a handful of extensions loaded with the Web PI version of PHP. Below is the list of extensions that are available to you when complete:

``` bash
[PHP_CURL]
extension=php_curl.dll
[PHP_GD2]
extension=php_gd2.dll
[PHP_GETTEXT]
extension=php_gettext.dll
[PHP_IMAP]
extension=php_imap.dll
[PHP_MBSTRING]
extension=php_mbstring.dll
[PHP_MSSQL]
extension=php_mssql.dll
[PHP_MYSQL]
extension=php_mysql.dll
[PHP_MYSQLI]
extension=php_mysqli.dll
[PHP_OPENSSL]
extension=php_openssl.dll
[PHP_PDO]
extension=php_pdo.dll
[PHP_PDO_MYSQL]
extension=php_pdo_mysql.dll
[PHP_PDO_SQLITE]
extension=php_pdo_sqlite.dll
[PHP_SOAP]
extension=php_soap.dll
[PHP_TIDY]
extension=php_tidy.dll
[PHP_XMLRPC]
extension=php_xmlrpc.dll
[PHP_ZIP]
extension=php_zip.dll
[PHP_EXIF]
extension=php_exif.dll
```
