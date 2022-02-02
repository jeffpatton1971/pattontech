---
layout: post
title:  "Views and the Cloud"
date:   2009-09-28 14:10:00 -0600
categories: blog
tags: September 2009 KU SOECS
---
One of the things I’ve enjoyed recently are tag clouds I have no good explanation for it other than they look cool. So since I just finished migrating all my junk over to this server I decided I would set that up. Drupal appears to have a module for almost everything so a tag cloud seemed like a no brainer on the module site listing. Sure enough there is the “[Views_Cloud](http://drupal.org/project/views_cloud)” module that depends on the “[Views](http://drupal.org/project/views)” module.

The module doesn’t really add anything other than a style to a view. Being completely new to this I started poking around the [documentation](http://drupal.org/node/351230). Which was pretty sparse, there was an exported “working” cloud and then a step-by-step on how to set your own cloud up. Both the export and the step-by-step yielded a page that had a really cool tag cloud that appeared to be functional, but when a tag was clicked on, you were directed to a page that had a link for each time the tag was used, not a list of the actual articles the tag could be found in.

Now, I noticed a few things, first the step-by-step suggests adding a Node Statistics: Page View Total, if you don’t have statistics turned on this isn’t around. I can’t remember if that’s a module or not, i don’t think so. Second that the path for my cloud was <http://www.patton-tech.com/cloud> and if I had clicked on the IT tag (id =1) I would get the following URL, <http://www.patton-tech.com/cloud/1>. At the time this worked but displayed literally one link for each time the tag had been used which was 98 times. So I had 98 links named IT and the url for each was the second URL I listed.

Since the IT tag’s ID is 1, I simply changed the URL in the address bar, replaced /cloud/ with /taxonomy/term/ and noticed that the new URL worked as it should. So all I needed to do is find out where the /cloud/ was being generated from. Turns out that in the Page “tab” for the view there is a path property that is set to “cloud” changing that to “taxonomy/term” my cloud works!

Hopefully someone will find this information useful.
