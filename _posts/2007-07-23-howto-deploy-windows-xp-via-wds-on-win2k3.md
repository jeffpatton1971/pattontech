---
layout: post
title:  "HOWTO: Deploy Windows XP via Windows Deployment Services on Windows Server 2003"
date:   2007-07-23 09:06:00 -0600
categories: KU SOECS Lab HOWTO
tags: July 2007
---
### Overview

The ability to deploy an OS via the network has been an important part of network administration for many years. Microsoft has provided tools to facilitate network installation since Windows NT 4.0, and with each evolution of the Operating System the tools have gotten more sophisticated and feature rich.

Remote Installation Services was introduced with Windows 2000 and it leveraged the new way of creating a Windows Network, Active Directory. It provided a much easier method of deploying the Windows OS to client machines, and the addition of “Plug and Play” to the core Windows OS, made it much easier to push the OS to disparate hardware.

With the release of Windows Vista a new way of deploying the Operating System is available. Windows Deployment Services, this can run within a WindowsServer 2003 box, or you can configure the WDS Role in Windows Server 2008. One of the coolest features is the new imaging format, .wim.

A windows image file can be mounted in the same fashion you might mount an ISO file. Once mounted you are able to browse the file and add/remove files from it, the changes can then be committed when the image is unmounted. Not only are you able to “browse” the file the implication is that you could even deploy a service pack to the OS by simply mounting the .wim file, or possibly even installing an application into the .wim file. Another exciting feature is the ability for the file to contain more than one image within it.

An administrator can have one “image” and within that image can have images for each department, or function group within the organization. Like it’s predecessor’s this is a free add-on to the OS, there is no additional licensing requirements for running WDS, there is no additional charge for installing the role in Windows. It’s free to use to anyone who has the knowledge to leverage the technology.

### Network Configuration

A minimum of two servers are required, one server will be the Domain Controller, DNS Server and DHCP Server. Technically the DHCP Server can reside on the WDS server, in fact it’s much easier now to have WDS and DHCP on the same server than it was under RIS. I configure it separate from WDS strictly due to habit out of using RIS.

Once your servers are installed, add the DNS, and DHCP services to the server that will become the Domain Controller. Configure DNS with the FQDN of your AD Installation, don’t worry that you haven’t run DCPROMO yet. Again, out of habit I configure DNS first due to an issue with Windows 2000 AD that has since been resolved.

Verify that DNS allows dynamic updates to both your forward and reverse lookup zones, then configure the DHCP service. Nothing special is needed for either DNS or DHCP, although I have taken to using exclusion ranges for RIS so only the machines I want to RIS have the ability to do so. Verify that your domain suffix is the FQDN of your AD Domain, verify that your Primary DNS is the IP Address of your soon-to-be Domain Controller, reboot the server.

Run DCPROMO and configure your AD Domain the same as the FQDN you configured in your DNS service. Reboot the server after the wizard is done and AD is successfully installed on your server. There is very little left to do on the Domain Controller once the AD Service has been installed.

### Server Configuration

Configuring RIS is very straightforward, simply install the service on the second server, the only real requirement is a second drive in this box. It should be large enough to hold all the images you would like to make available, I will walk through configuring RIS as it is extremely simple.

Once the service is installed you are required to reboot the server, when it comes back up complete the setup by providing the first distribution image, it may be a Windows Server 2003 image or a Windows XP image, you could probably get away with a Windows 2000 Professional image, but I’ve never tried that. Then I usually run “adminpak.msi” to install the Windows Server 2003 Administrative Tools on the RIS Server. This allows you to open “Active Directory Users and Computers” so that you may configure the RIS Server itself.

First things, authorize the DHCP Server, you will be able to do this via the DHCP MMC, under Administrative Tools. You should be able to authorize the RIS server in the same fashion, if you have problems attaching to a PXE client to the RIS server then rebooting the RIS server usually resolves this problem, your mileage may vary.

Configuring WDS is even simpler, install Service Pack 2 on the RIS server, reboot the server, open the Windows Deployment Services console from “Administrative Tools” and add your server to the list. Once the server has been added to the list, select your server and then choose to “Configure the server”, the most important part of this is specifying the location of the Remote Installation directory.

Your RIS server is now set to deploy regular RIPrep images as well as the new .wim format for Windows Vista and later Operating Systems.

### RIS vs WDS

At a high level, both of these products do the exact same thing, take an “image” from the server and put it on the client. How this is accomplished by both is a little different.

RIS copies all the files from the server to the client and setup runs and configures the client machine and if you have scripted it via an unattend file, then it is totally hands-free. I’ve done some monitoring of a typical RIS session, network utilization goes up considerably but not the point to which it overwhelms or floods the network. A complete installation of Windows XP SP 2 took 27 minutes to complete, from beginning to end on a 100mb switched network.

I’ve not done the same monitoring of WDS as I did for RIS, but what I do notice is this install is more like a Windows Vista install than anything else. The image is deployed directly to the hard-drive, there is no setup to walk through, and any configuration there is could be scripted as well.

During a RIS install the clients each connect to the distribution share and download each file separately, whereas with WDS, the image file is accessed directly over the network. Fewer open files on the server, but the files are larger, overall I don’t see any appreciable difference performance-wise between the two.

### What to do with RipRep Images

It is possible to convert RipRep images to the .wim format using the wdsutil command, it’s very straightforward, and has a very rich help system associated with it.

wdsutil /? gives help about the utility

wdsutil /Convert /? gives specific help about the convert option

wdsutil /Convert-Riprepimage /? gives help about converting RipRep images

wdsutil /Convert-Riprepimage /Filepath:”RemoteInstallSetupEnglishImagesXP-Base-WithUpdatesi386Templatesriprep.sif” /DestinationImage
/Filepath:”RemoteInstallImagesXP-Base-WithUpdates.wim”

This converts a RipRep image of Windows XP with updates from the default location to a .wim file in a different directory, there are several options that go along with the process, I urge you to use the built-in help system to explore the commands available.

### Capture vs Boot Images

Boot images allow a pxe client to boot and connect to a WDS server to deploy an image, or run some other utility. There are some interesting possibilities with WinPE. There are a variety of boot images you can choose from, there is a boot image on the WAIK, you can use an OEM boot image, or the boot image from a regular distribution DVD.

Capture images are created from boot images and allow the administrator to take a “snapshot” of a currently installed system and create a .wim file locally on the reference machine and optionally on the WDS Server itself.

Regardless of which you choose creating a boot image is the same:

1. Right click on boot images in the WDS Console
2. Choose “Add Boot Image”
3. Browse to the location of the .wim file you wish to use
4. Provide an Image Name and Description
5. Verify that everything is correct and click Finish

The winpe.wim from the WAIK lacks setup.exe and the supporting files that allow setup to function, but it does include everything needed to create acapture image.

The boot.wim from an OEM DVD is locked to only deploy from DVD, the file you are interested in is pid.txt which is used for filtering.

### Creating a Capture Image

To create a capture image you must have a boot image first. The capture image is based on the boot image available to the server. Once you have a working boot image, then creating a capture image is very straightforward:

1. Right click on the boot image
2. Choose “Create Capture Boot Image”
3. Provide an Image Name and Description
4. You must also provide a location to store the file
  a. The default location is RemoteInstallbootx86Images
5. Clicking Next starts the capture image creation process
