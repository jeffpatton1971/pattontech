---
layout: post
title:  "The 'new way' or Software Lifecycles"
date:   2008-03-06 11:00:06 -0600
categories: blog
tags: March 2008 KU SOECS
comments: false
---
Currently we have a very poor methodology of managing the host of software applications that we support on our network. The process is as follows:

New Software Request

Faculty: I need application xyz.exe installed for my class

Staff: Ok

Student: Application xyz.exe doesn’t work, and I need this for a very important project

Staff: Ok

Faculty: Why doesn’t this application work, I can download it and run just fine on my computer at home.

Staff: Ok

Software Upgrade

Faculty: The latest release of xyz.exe is out today, we need this loaded on the labs.

Staff: Ok

Student: Application abc.exe doesn’t work anymore, and I can’t get xyz.exe to run widget.

Staff: Ok

Faculty: I don’t know why abc.exe doesn’t work, I don’t have that problem on my computer at home.

Staff: Ok

Do you see the problem? We have no formal process of evaluating the impact of a given application in our environment, and beyond that once we have a given application working, we have no procedure on how to deliver that software to the appropriate computers. This is where Software Lifecycle comes in to play. All software, regardless of it being new, or an upgrade to existing applications will go through the following process.

Step 1.

Install the application onto a test machine. The test machine will have the base load of software, consisting of Windows, Office and Sophos Anti-Virus.

Step 2.

Verify that the application will start, then have someone who is knowledgeable in the program to verify that it works under a regular user context. If there is a problem, research and resolve that problem and then test again. This process will continue until the application works.

Once the application has been verified to be functional it will then be packaged into an .msi file and installed onto a target machine. The target machine will exactly mirror the targeted lab environment in every single way.

Step 3.

Verify that the application will start, then have someone who is knowledgeable in the program verify its functionality. Document any issues that occur at this point, and resolve them. This process will continue until the application works. Once the application is functional then all applications that are already installed will be verified to work, if any issues are discovered document and resolve them.

Once the target machine functions any fixes will be packaged into the application prior to deployment.

Step 4.

Load the packaged application onto the distribution server, and configure the appropriate software deployment policy.

Another option would be to use System Center Configuration Manager to schedule the deployment of the application at a time when it will have the least impact on the lab and the students who may be using the lab. The benefit of this solution is the ability to schedule when an application gets installed as opposed to waiting for a computer reboot to occur.

Once this system is in place we can begin to develop a valuable documentation resource for our applications.
