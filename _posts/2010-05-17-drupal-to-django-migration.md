---
layout: post
title:  "Drupal to Django Migration"
date:   2010-05-17 19:13:00 -0600
categories: blog
tags: May 2010 Personal
comments: false
---
To be honest I completely ripped off Carson, he has been talking on and off about this for a while. I decided about a week ago that I would give it a stab, and so started searching Google for answers. Surprisingly there weren’t too many answers that could be found that worked or made sense. So I decided the first thing I needed to know how to do was setup a site with Django. Two friends of mine, Nick and Carson are really amazing developers. Nick wrote a Django blog app that we use at the School of Engineering and Carson wrote a Django form app that we use as well. So I figured I’d build a site using Nick’s blog and Carson’s forms and if I got stuck pester them until they helped me figure out what I messed up.

I am most definitely a Windows guy, and I’m really not ashamed of it, but I also really enjoy doing things just outside my comfort zone. Drupal was about as easy a setup as you could get and has worked well and I’ve had no complaints, but I’m bored with it now. So I started reading and messing around with stuff on my laptop. I started out setting up a VM with Ubuntu 10.04 Server, then I proceeded to install the following packages:

sudo apt-get install apache2 libapache2-mod-wsgi python-imaging subversion mysql-server-5.1 python-mysqldb

That got me nearly everything I needed, Ubuntu 10.04 already can with Python 2.6, so I just needed to grab the web server, database server and supporting libraries. I believe on the VPS host I’m using I need one of the Apache2-mpm packages but I haven’t checked that yet. The next thing I did was grab the code I needed. So because Nick and Carson keep giving me a hard time about it I’ll be grabbing the release version of Django 1.1.1, the most recent release of Nick’s blog and the most recent release of Carson’s formunculous.

wget <http://www.djangoproject.com/download/1.1.1/tarball/>

wget <http://sourceforge.net/projects/formunculous/files/formunculous-2.1.0-final.tar.gz/download>

wget <http://code.photodwarf.org/downloads/python/django-blog/django-blog-1.2.tar.gz>

After you extract them into their respective directories you will want to install each. You need to run the following command from within each project’s directory. You may get a prompt during the Formunculous install that you will need some additional tools, don’t worry it will grab them for you.

sudo python setup.py install

There are several different ways to lay out your sites, and the one I seem to have settled on is as follows:

/sites

/site-1

/scripts

/htdocs

/media

/templates

/site-2

The way I’m going to progress from here is in three parts. The first part involves setting up a dev site, a database for it and then importing your database from drupal into the blank dev database. I’m specifically working with MySQL but I would imagine it would be similar steps if you were working with Postgres. The second part will be copying the data from the drupal tables into the Django tables, again I’m specifically looking at my blog posts as well as my books. Finally dumping the Django tables out to a file, creating a fresh database for Django and importing those files into the new database. It may be a lot of extra work, but I think in the end it’s worth it. You will have a backup of your complete drupal site that you can store somewhere, and the new Django site will have only the bits you want.

Part 1. Setting up dev

I usually create the /sites folder first then each site-n folder is actually a Django app. So once you are ready issue the following command:

sudo djangoadmin.py startproject drupalimport

Then you can create the rest of your directory structure:

sudo mkdir scripts templates htdocs htdocs/media

Create a backup of your Drupal database so you’re not working with production data. I decided to create my backup inside a folder called drupal that was within my ‘drupalimport’ app. This folder will become an ‘app’ in order to bring it into Django.

sudo mkdir /sites/drupalimport/drupal

sudo chmod 777 /sites/drupalimport/drupal

sudo mysqldump drupaldb > drupaldb.sql -p

sudo touch drupal/__init__.py

* Alternatively you might also consider install the Backup-Migrate Drupal module and use the backup .sql file it generates

* If you don’t chmod 777 you will get permission denied when you attempt to write your sql file

You will want an interim database to use while you perform the migration. You will import your Dupal data into it and use it as your Django database as well. This will make it easy to copy the information from the Drupal tables to the Django tables that you have.

sudo mysqladmin create djangodb -p

Import the Drupal data into the newly created Django db:

sudo mysql djangodb < drupaldb.sql -p

You will want to setup your Django site to see the blog and admin apps at the very least. You will also want to enable the admin urls and blog urls so you can verify everything is working properly. In order to migrate the data from Drupal you need to sync the database so Django builds in the appropriate tables.

sudo python manage.py syncdb

You should now be ready to begin the data migration.

Part 2. Python Magic

Carson helped me work this all out, as I am in no way comfortable using Python. Django needs to model the database so that it can manage it.

sudo python manage.py inspectdb > drupal/models.py

This will create a models.py file that should contain a list of all the tables and fields. In our experience we noted that the Node table has a nid field which is a key field. In order to actually get this to work we had to modify a line in the models.py file. Open the file and search for the following string, “class Node” this will take you to where you need to be, and change the nid line from:

nid = models.IntegerField()

To:

nid = models.IntegerField(primary_key=True)

Save and close the file, now we can bust out the Python shell magic:

sudo Python manage.py shell

You will need to import the the Drupal model, the blog model and the date and time:

from drupal.models import *

from blog.models import *

import datetime

Next you need to create yourself an author:

author=Author()

author.name=”Jeff Patton”

author.email=”jeffrey@patton-tech.com”

author.ident=”jeff-patton”

author.save()

Set the site so the blog entries will show up:

site=Site.objects.all()[0]

* Don’t worry what this is you can change it later

I decided I wanted to tag all these entries so I could differentiate between the Drupal blog entries, Drupal book entries and so on:

tag=Tag()

tag.tag=”Drupalblog”

tag.save()

My example will show you how to pull the blog entries from the Drupal tables, you can use the same method to pull whatever information you need out. The two tables we are interested in appear to be Node and NodeRevisions. Node contains nid, title, and so on whereas NodeRevisions contains the actual body of the post/node. In order to pull them out we need to create an object of just blog entries and loop through it:

nodes=Node.objects.filter(type=”blog”)

That’s really all there is, you would substitute blog for whatever you were interested in, such as:

blog

geshinode

book

page

* These are the nodes I have, you may see different nodes and should looke at Node.type to see what’s available in your installation

Now that we our object setup we need to loop through it and pull out a particular blog article (nid) and store it as an entry:

for node in nodes:

entry=Entry()

entry.pub_date=datetime.datetime.now()

entry.slug=node.nid

entry.headline=node.title

entry.author=author

entry.abstract=NodeRevisions.objects.get(nid=node.nid).teaser

entry.content=NodeRevisions.objects.get(nid=node.nid).body

entry.save()

entry.sites.add(site)

entry.tags.add(tag)

Hit enter a couple times after this and it should go. Now for the fun part! Login to your admin site and you should see all the entries fully populated. I haven’t quite worked out the tags yet, but here are a few things you could do. First all the tags in Drupal are stored in Term_Data.Name, so you could just add them all in manually, or in a loop like above. If you want to be all fancy then here is the gist of what you need to understand.

There are basically three tables we are interested in; Node, Term_Node and Term_Data. Term_Node is a junction table between Node and Term_Data. There is a one-to-one between Node.nid and Term_Node.nid, there is also a one-to-one between Term_Node.tid and Term_Data.tid. My guess at this point would be that in place of entry.tags.add(tag), there would be a for loop akin to the following.

For each Term_Node.tid where Node.nid = Term_Node.nid entry.tags.add(Term_Data.name)

If you receive an error let me know I’ve run into a couple of issues:

Index out of range: Resulted in trying to add the site to the entry before the site entry was created (saved) which is why the save statement is where it’s at.

Permission Denied: When doing the ‘mysqldump’ into a folder that wasn’t chmod’d 777

Part 3. Setting up prod

Once you have migrated your data from Drupal into Django you may want to have a fresh database to work with. That is to say a database that has no Drupal information within it. The nice thing about this process is that the .sql file that you created earlier will serve as a backup for your Drupal site. If at some point you decide to go back to Drupal, it’s simply a matter of importing that .sql file into a Drupal database of the same name and specifying that during the Drupal installation.

We will use ‘mysqldump’ to export out the tables we’re interested in to a .sql file and then create our fresh database, import the sql file and point our new site at our clean database. The tables we’re interested in are below:

blog_author

blog_entry

blog_entry_sites

blog_entry_tags

blog_tag

Now create your backup:

sudo mysqldump djangodb blog_author -p > blog.sql

sudo mysqldump djangodb blog_entry -p >> blog.sql

sudo mysqldump djangodb blog_entry_sites -p >> blog.sql

sudo mysqldump djangodb blog_entry_tags -p >> blog.sql

sudo mysqldump djangodb blog_tag -p >> blog.sql

* See note in Part 1 regarding chmod

You now have the .sql file that you can insert into a fresh database:

sudo mysqladmin create djangosite -p

sudo mysqldump djangosite < blog.sql -p

sudo djangoadmin.py startproject djangosite

sudo mkdir scripts templates htdocs htdocs/media

Configure your site to point at your new database, configure the urls and update your Apache configuration and you should be good to go!

Attached is the import file that I created with Carson’s help. I had an issue where I didn’t have my Django variables setup properly for it to just run. So I opened a shell and pasted it there and it just worked. I’m sure I’ll have missed something, so let me know if there is a problem!
