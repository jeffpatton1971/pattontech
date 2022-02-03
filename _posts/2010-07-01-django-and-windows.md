---
layout: post
title:  "Django and Windows"
date:   2010-07-01 19:17:00 -0600
categories: blog
tags: July 2010 Personal
---
You will need several things before you begin:

* [7-zip](http://www.7-zip.org/)
* [Python](http://python.org/download/)
* [MySQL Server](http://www.mysql.com/downloads/mysql/)
* [Setuptools](http://pypi.python.org/pypi/setuptools/)
* [Visual Studio 2008 Express: vcsetup.exe](http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=a22341ee-21db-43aa-8431-40be78461ee0)
* [MySQLdb Module](http://sourceforge.net/projects/mysql-python/)
* [Apache](http://httpd.apache.org/download.cgi/)
* [Mod WSGI](http://code.google.com/p/modwsgi/wiki/InstallationOnWindows)
* [Sysinterals Suite](http://technet.microsoft.com/en-us/sysinternals/bb842062.aspx)
* [Django](http://www.djangoproject.com/download/)

### GETTING STARTED

The above links should let you download everything you will need to get started. I typically download these to the downloads folder and run them from there. Sysinternals and Django are zipped up so you will need 7-zip to extract them. You should be able to install them more or less in the order above. One note about the MySQL install, I don’t choose the Essentials, I tend to grab the full msi.

### SETTING EVERYTHING UP

Python requires very little as the installer does most of the work for you. I noticed for me that I had to set C:Python26 in my path in order for it work. You may also want to add the C:Python26tools directory to your path as well.

Make sure that you grab the correct version of setuptools to match the version of Python you downloaded earlier. Once you have the proper version the installer will get everything set for you. You will notice that mysqldb is a gzipped tarball so extract it all out into a folder so you can get it going. You will need to have Visual Studio 2008 Express C++ installed and copy the VCVARSALL.BAT file into the folder mysqldb extracted out to. This part requires some effort, but I found a very nice [article](http://www.fuyun.org/2009/12/install-mysql-for-python-on-windows/) that details how to get it going. Following his instructions should get you where you need to be.

If you happen to get stuck with error messages like:

> VCVARSALL.BAT Missing
> command 'mt.exe' failed with exit status 31

Then you should go back to the article aboe and read down through the comments. The good part is halfway down the [page](http://www.fuyun.org/2009/12/install-mysql-for-python-on-windows/#comment-150). It links to a file that has an installer, if you get stuck you may just try and download and install this file.

I choose the complete option when setting up MySQL so that I have no surprises later. During the post-setup I configure the following options:

* Install as Windows Service, autostart
* Include on path
* Define a root password

If you want your MySQL instance to be available on the network, you will need to make some additional modifications. Reconfigure your service and choose detailed configuration, and click through until you get to networking. Make sure that you have checked:

* Enable TCP/IP Networking
* Add firewall exception for this port

Apache configuration is more or less automatic, as the installer takes care of everything for you. In order for Django to work properly though, you will need to load the WSGI module. You should be able to just copy or move the mod_wsgi-win32*.so file into the modules folder wherever Apache was installed. Once that is done simply add the following line to your Apache conf:

``` dos
LoadModule wsgi_module modules/mod_wsgi-win32*.so
```

Replace the asterick with whatever the full name of the file you downloaded is.

You may have noticed that there isn’t really an installer for Django or Sysinternals. You will need to extract both of those out to their respective directories. As Django is a gzipped tarball, I use 7-zip as it can extract it out quite nicely. From there you can copy the contents of the sysinternals folder directly to %SYSTEMROOT%System32. For Django you will need to have set the %PATH% variable to point at the python direcory. After that you just need to run:

``` dos
python setup.py install
```

### DJANGO SETUP

For information on how to use Django I suggest you read the [documentation](http://docs.djangoproject.com/en/1.2/) available on the Django Site.

The first thing you need to do is get your Django project (site) started.

``` dos
django-admin startproject project
```

I have a particular way I lay out my site, so if you do it differently you will need to modify your site’s conf file later on.

``` dos
cd sites
md htdocs
md htdocsmedia
md scripts
md logs
md templates
```

Now is where we need the Sysinternals stuff, and technically we just need one utility, junction, but the rest are certainly worth while to have. The junction utility allows you to create symlinks in Windows that are akin to what you would have in Linux. While not as robust as ln, junction will do just what we need. We want to link the media folder from django.contrib.admin to a folder inside our project’s media folder.

``` dos
junction -s c:sitesprojecthtdocsmediaadmin_media c:python26libsite-packagesdjangocontribadminmedia
```

You should now have a “folder” called admin_media inside the media folder of your project site. If you would like more information about junction, I would suggest you read through the Sysinternals site from the link at the top.

Inside your project’s folder you will see a settings.py file, and that file is where you define the settings for your site. I will show you the basic stuff to get you going.

The ADMINS section is where you define the administrator of the project, make sure to remove the comment in front if you ever want your site to email you. The DATABASES section is where you define the driver, database, user/pass, host and port information so your site can talk to a database. These are the basic things that need to be configured in order for your site to work.

Additionally you can define the location of your MEDIA_ROOT and MEDIA_URL, if you glance down to the server setup you will see these mapped in your apache conf. The ADMIN_MEDIA_PREFIX is also defined in your apache conf, this is also the folder that we symlinked in earlier. Your TEMPLATE_DIRS is the path to templates folder you created above, this is not available through Apache. Finally, if you want to be able to manage your site, you want to uncomment the django.contrib.admin line in INSTALLED_APPS.

Technically your site is done and you can run the following commands from your project directory to get it started:

``` dos
python manage.py syncdb
```

There are some basic questions you will be prompted to answer and then the site should be nearly done. You can check by running this command:

``` dos
python manage.py runserver 0.0.0.0:81
```

Assuming you received no errors, point your web browser at the IP of your server at port 81, and you should see the pleasant, “Welcome to Django” page. If you get a yellow screen, something went wrong, but don’t worry usually it’s something silly and the screen will point you at the line in question.

While it’s cool that you can just run your server this way, we didn’t spend all that time downloading and install Apache for nothing. If you want to access your site using Apache you will want to read the section below, and potentially the last thing you need to do in Django before you start poking around is setting up your urls. There is a urls.py file in the same folder as the settings.py file. Inside this file you need to uncomment the lines pertaining to the admin site, don’t worry it tells you which lines to look at.

### SERVER SETUP

In order for Apache to server your site you need a conf file for it. I created a folder called C:sites, and inside that folder I created a file called project.conf. Using whatever editor you like you need to fill in your file with some basic information. This is what mine looks like:

``` dos
ServerAdmin jspatton@ku.edu
DocumentRoot c:/sites/project/htdocs
Alias /admin_media c:/sites/project/htdocs/media/admin_media
Order allow,deny
Allow from all
Alias /media c:/sites/project/htdocs/media
Order allow,deny
Allow from all
WSGIScriptAlias / c:/sites/project/scripts/project.wsgi
Order allow,deny
Allow from all
LogLevel warn
CustomLog c:/sites/project/logs/apache.log combined
```

You now need to include that file in your httpd.conf file on Apache. There is a Start menu shortcut to edit that file in Windows. Once you are in the file, scroll to the bottom and place this line there:

``` dos
Include c:/sites/project.conf
```

In order for Django to work it needs a database connection and since we went ahead and installed MySQL we should set it up. You have access to the MySQL command-line, fire that up and enter your root password from your earlier configuration. We will enter our commands at the mysql> prompt.

To create a database enter the following command:

``` dos
create database project;
```

To create a user account for your project:

``` dos
create user projman identified by ‘Pass12345’;
grant all on *.* to ‘projmon’@’%’;
```

Since I don’t want to cover the fine grained access controls that are available to you in MySQL. I would suggest you point your browser to the [MySQL documentation site](http://dev.mysql.com/doc/refman/5.1/en/).

### STARTING THE SITE

Assuming you didn’t have any errors as you were following along, you should now be able to restart the Apache service and view your site. When you first go to your site, you may see a yellow screen, that’s ok you have left Debug=True in your settings file, but everything is working. Point your browser to the admin directory and you should see the pretty green login screen. If you don’t see a small box with a green header that reads Django Administration, check your symlink, settings file, apache conf or urls.py file. Otherwise, login with the username and password you defined earlier when you ran syncdb, and you should now be in Site Administration.

We’re all done, Django is now running on Windows served by Apache and mod_wsgi. Good luck!
