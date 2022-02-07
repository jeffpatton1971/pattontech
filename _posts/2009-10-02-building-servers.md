---
layout: post
title:  "Building Server"
date:   2009-10-02 14:13:00 -0600
categories: blog
tags: October 2009 KU SOECS
---
Building a server, using the method I outlined in my [previous post]({% post_url 2009-10-02-windows-linux-server-base-image %}), becomes cake.

Building a server:

1. Create a new virtual machine, use the default option
2. Configure the settings for your new VM
3. Run the New Virtual Disk wizard
   1. New virtual disk
   2. New Hard Disk
   3. Browse to the folder where your VM is stored
   4. Choose differencing disk
   5. Browse to the folder where the Server Base Image VHD is located
4. Add the disk to your VM, make sure to browse to the proper VHD
5. Power on the VM

Depending on how long ago the base image was created you may wish to check the OS for any security updates to make sure you’re as patched as possible.
