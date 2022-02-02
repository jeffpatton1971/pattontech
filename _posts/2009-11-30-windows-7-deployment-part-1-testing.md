---
layout: post
title:  "Windows 7 Deployment: Part 1 Testing"
date:   2009-11-30 14:19:00 -0600
categories: blog
tags: November 2009 KU SOECS
---
We recently upgraded our System Center Configuration Manager (SCCM) server to SP2, and according to Carson everything went just fine, which is always good news. We didn’t have nearly as pleasant experience when we flipped the bits to SP1. This upgrade put’s us in a position to deploy Windows 7 to our computers.

The process is rather straightforward and you can find the outline below.

1. Install Windows 7 onto reference computer
2. Create updated SCCM capture media
3. Perform the capture
4. Upload WIM to Distribution Share
5. Import OS Image
6. Create Task Sequence
7. Create Advertisement
8. Publish to Distribution Points
9. Test

Install Windows 7 onto reference computer

This is arguably the easiest part of the entire process. Find your favorite SKU of Windows 7 and install it onto your reference machine. This will be the computer that you will configure with all appropriate software, updates, and whatever else may be needed by your organization. Time involved will depend on the amount of customization needed for your organization. For my test I installed Windows 7 Enterprise x86, I did not join it to the domain, nor did I perform any additional tweaking. It is the basic Windows 7 install and nothing more.

Create updated SCCM capture media

Again not a terribly complicated process, if you’ve used System Center before you already know how to do this:

1. Open SCCM Console
2. Expand Computer Management
3. Expand Operating System Deployment
4. Right click Task Sequences
5. Click, Create Task Sequence Media
6. Choose Capture media, choose USB or ISO
7. Select Boot Image
8. Click Finish

I chose the x86 boot image, but I don’t think that really matters much at all.

Perform the Capture

Capturing is fairly straightforward, for peace of mind you may want to capture your reference computer to either a local disk, or attached USB drive. There “could” be problems if you attempt to capture over the network, and while that eliminates one step I don’t know if it’s worth it. If you capture to the network make sure that you choose the Distribution Share as the folder to store the .WIM in.

Upload WIM to Distribution Share

This step is only necessary if you capture your reference computer to a local disk. You will need to copy and paste the .WIM file (~2GB) to the network location of your Distribution Share, that’s literally all that’s needed.

Import OS Image

This task sets up your custom .WIM in SCCM so you can deploy it to your clients.

1. Open SCCM Console
2. Expand Computer Management
3. Expand Operating System Deployment
4. Right click Operating System Images
5. Choose Add Operating System Image
6. You will need to enter the UNC and filename of your .WIM into the dialog
7. Give it a Name, Version and any Comments you like
8. Click Next until you’re done

Create Task Sequence

There are a couple of ways to do this, the easiest method is to Duplicate and existing Task Sequence and simply change what image you deploy, that is the method that I chose:

1. Open SCCM Console
2. Expand Computer Management
3. Expand Operating System Deployment
4. Click Task Sequences
5. Right click an existing OS Task Sequence and choose duplicate
6. Right click on the newly created Task Sequence and choose properties
7. On the General tab rename the Task Sequence
8. On the Advanced tab verify that the Boot Image is the correct architecture for your deployment
9. Click Ok
10. Right click on the newly created Task Sequence and choose Edit
11. Select the Apply Operating System Task
12. Choose your Windows 7 Image and click Ok

Create Advertisement

1. Open SCCM Console
2. Expand Computer Management
3. Expand Operating System Deployment
4. Click Task Sequences
5. Right click on your Windows 7 Task Sequence
6. Click Advertise
7. Choose which Collection to make it available to
8. Check the option to make it available to Boot Media and PXE clients
9. Define a schedule if you need to
10. Choose your Distribution method
11. Choose the level of end-user interaction
12. Change the Security if needed
13. Click Next until it’s done

Publish to Distribution Points

This is the step I usually forget, as you can see there are several things that need to be configured and it’s pretty easy to miss a couple.

1. Open SCCM Console
2. Expand Computer Management
3. Expand Operating System Deployment
4. Expand your Windows 7 Image
5. Right click Distribution Points
6. Click New Distribution Points
7. Choose which Distribution Point(s) to use
8. Click Next

Test

I would suggest that you test your deployment as many different ways as possible. I tested deploying via a burned image, as well as via PXE our two most common methods of deployment.

I hope this was helpful!
