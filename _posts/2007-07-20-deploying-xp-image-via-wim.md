---
layout: post
title:  "Deploying XP Image via WIM"
date:   2007-07-20 09:04:00 -0600
categories: KU SOECS Lab
tags: July 2007
---
Steps:

1. Install Windows XP via any means
2. Install and configure applications
3. Create a WDSCapture image
4. PXE boot remote client
5. Choose the Capture Image
6. Specify a local drive/filename for the image
7. Specify the WDS Server and Image Group

Create WDSCapture Image:

1. Open WDS MMC on server
2. Right click on a valid boot image
   - Some discussion/confusion on newsgroups that a WAIK boot image won’t allow an install/capture
3. Choose “Create a capture image”
4. Specify a name/location

Additional Notes:

The boot image that you create can be from an OEM copy of Vista, but if so you must removed the pid.txt file from inside the image. Alternatively if you have access to a regular distribution DVD, then you can use the boot.wim from there.
