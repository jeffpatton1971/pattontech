---
layout: post
title:  "Customized DOS Shell"
date:   2011-02-22 13:04:00 -0600
categories: blog
tags: February 2011 Personal
---
A fair amount of my time is spent at the command-line, and as such I want it to work in a particular way. Since I don’t want to lose this information the next time I need to set this up, I figured I’d write it down someplace.

![image_1](https://prdwebappstorage.blob.core.windows.net/pattontech/images/image_1.png)

The console window

I hate the console window, it’s hurty and painful. By default you can’t copy/paste easily, and the size of the screen is set to 80 cols. Nobody has a screen anymore like that so I wanted that to change first. You need to make changes to the default preferences for the console window. Open the command prompt and right click on the title bar and choose, “Properties.”

![image_2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/image_2.png)

Command History:

* Buffer Size: 999
* Number of Buffers: 5
* Discard Old Duplicates: True

Edit Options

* QuickEdit Mode: True
* Insert Mode: True

![image_3](https://prdwebappstorage.blob.core.windows.net/pattontech/images/image_3.png)

Screen Buffer Size:

* Width: 2500

Window Size:

* Width: nnn
* Height: nnn

Window Position:

* Left: nnn
* Top: nnn
* Let system position window: False

Where nnn is some number that works for you.

Command Prompt

The usual prompt we’re all familiar with C:WindowsSystem32>_ it’s ok, it tells you where you are, but how much fun is that? So I changed it to something that is a little more descriptive. I wanted it to look a little more like a linux prompt, plus give me more command-line space.

``` dos
jspatton@l1132c-pc01 | 16:05:15.33 | Tue 2/22/2011 | C:Windows
```

In order to get this prompt all I did was run the following command:

``` dos
prompt=%username%@%computername% $B $T $B $D $B $P $_
```

The only problem with this command is that as soon as you exit the shell it goes away. So to make it permanent change the syntax of the command above to the following:

``` dos
setx prompt "%username%@%computername% $B $T $B $D $B $P $_"
```

Setx will store that value in the registry which will allow your prompt to persist across multiple shells. Now to make it more fun you need to download a few things, first since I tend to find myself in Linux and Windows on a regular basis I sometimes find myself issuing linux commands in Windows. For a list of all the prompt variables you can use, visit [ss64.com](http://ss64.com/nt/prompt.html). In addition to that you also have access to all the regular [environment variables](http://technet.microsoft.com/en-us/library/cc737438%28WS.10%29.aspx). GnuWin32 has a whole suite of linux commands that you can download and install, but it’s a pain so someone created a project that downloads and installs all the tools for you, [GetGnuWin32](http://getgnuwin32.sourceforge.net/). I won’t go into details on how to install this, read the page it gives you what you need.

What would be nice right now is ANSI.SYS but that hasn’t been around for ages, so if you want to have nifty colors in your prompt you will need something similar. There may be several utilities that will do this for you, but I found [ANSICON](http://adoxa.110mb.com/ansicon/index.html), it’s pretty straightforward, download and extract the files onto your computer. Once you’ve done that all that’s left is to load it into the registry with the following command:

``` dos
ansicon –i
```

That’s about it, now I have posted up here and you know how to do it too!

Enjoy!
