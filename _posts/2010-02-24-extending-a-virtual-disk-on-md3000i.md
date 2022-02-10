---
layout: post
title:  "Extending a Virtual Disk on the MD3000i"
date:   2010-02-24 19:04:00 -0600
categories: blog
tags: February 2010 KU SOECS
comments: false
---
We purchased our new iSCSI [SAN]({% post_url 2009-06-06-dell-md3000i %}) this past summer and one of the important features for us was the ability to resize a lun as needed. The Dell MD3000i gave us that capability, while not in the best format, it does have a rather extensive CLI.

The CLI tools are only available to you if you install the management software on your computer. Once you have that installed, it might be a good idea to add the following to your PATH statement, it makes things easier.

``` dos
C:Program FilesDellMD Storage Managerclient;
```

From that point on it is rather straightforward, first you will need to connect to your array:

``` dos
SMCLI fqdn.of.yourarray
```

Then to avoid being kicked out of the CLI everytime you type something wrong you may want to execute the following command. You should note that all commands MUST end with a semicolon.

``` dos
set session errorAction=continue password=”Array Password”;
```

Once you are logged in it’s a simple matter of knowing which set of commands to use. Depending on the version of array you have you may need to make some changes, but it should look something very similar to the following:

``` dos
set virtualDisk [“Name of Virtual Disk”] addCapacity=1TB;
```

This command would increase the capacity of your viurtal disk by 1TB. This is assuming you have 1TB unused to add, if not you will need to add additional drives or enclosures to your array. I’m providing links to the documentation for both your sake and my own.

* [Dell MD3000i Documenation](http://support.dell.com/support/edocs/systems/md3000i/en/index.htm)
* [Dell MD3000i First Gen User Documentation](http://support.dell.com/support/edocs/systems/md3000i/en/1stGen/index.htm)
* [Dell MD3000i Second Gen User Documentation](http://support.dell.com/support/edocs/systems/md3000i/en/2ndGen/index.htm)
