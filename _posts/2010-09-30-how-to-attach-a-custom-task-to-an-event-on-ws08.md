---
layout: post
title:  "How to attach a custom task to an event on WS08"
date:   2010-09-30 19:30:00 -0600
categories: blog
tags: September 2010 KU SOECS
comments: false
---
In an earlier [post]({% post_url 2010-09-22-forwarding-events-from-ws08-core-dc %}) I talked about how to forward events from one machine to another. That works out quite well, but unless you’re sitting at the log and watching the events go by, you might miss something. You might agree that there are some events that you are more interested in than others. For example, while it’s neat that Windows let’s you know server uptime in seconds as an event but I’d be much more interested in an event related to a computer that is unable to establish a trust relationship with it’s domain controller.

> For details on [Event 5722](http://www.microsoft.com/technet/support/ee/transform.aspx?ProdName=Windows+Operating+System&ProdVer=5.0&EvtID=5722&EvtSrc=NetLogon&LCID=1033)

The nice thing about Windows Server is that you can set up a custom task to let you know when a particular event occurs. This doesn’t really replace any sort of quality monitoring software like System Center Operations Manager or Zenoss, but it’s most certainly a nice thing to have built-in. So how do you go about setting this up? In my example I’ll be creating a task that sends me an email based on an event forwarded from my domain controller. It will require a script, but I’ll include it for you.

First thing is to have a script that writes the event to a file. The reason is there is sadly no built-in feature to pull the event details and put it into an email task. So we’ll have two tasks that run, one that runs the script to make the file. The second to take that file as an attachment and email it to you.

``` visualbasic
Option Explicit

Const ForWriting = 2
Dim strComputer
Dim strEventCode
Dim objWMIService
Dim objEvents
Dim objEvent
Dim objFSO
Dim objFile

strComputer = “.”
strEventCode = CInt(Wscript.Arguments.Item(0))

Set objWMIService = GetObject(“winmgmts:{(Security)}\” & strComputer & “rootcimv2”)
Set objEvents = objWMIService.ExecQuery(“Select * from Win32_NTLogEvent where logfile=’system'”)
Set objFSO = CreateObject(“Scripting.FileSystemObject”)

If objFSO.FileExists(“C:scripts” & strEventCode & “.txt”) Then
    Set objFile = objFSO.OpenTextFile(“C:scripts” & strEventCode & “.txt”, ForWriting)
Else
    Set objFile = objFSO.CreateTextFile(“C:scripts” & strEventCode & “.txt”)
End If

For Each objEvent in objEvents
    If objEvent.eventcode = strEventCode Then
        objFile.WriteLine objEvent.eventtype
        objFile.WriteLine objEvent.eventcode
        objFile.WriteLine objEvent.message
        Wscript.Quit
    End If
Next
```

The script above is very simple. It accepts a single command-line parameter, the event code. It connects to the WMI service of the local computer and pulls in the System log. Then it checks to see if there is a file named for the event code in the scripts directory, if not it creates one, otherwise it overwrites the existing one. Then inside the For Each loop, we find the first event code that matches the provided code, and write out the type, code and message and then exit out of the script altogether.

Now we need to find a task that we are interested in getting notified about, and attach a task to it. The first thing you need to do is head over to the event viewer, and right-click on a task.

![20100930-attachtask][(](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100930-attachtask.png))

Fortunately a wizard will guide you through the entire process. The first thing you need to do is provide a name for your task that makes sense for you.

![20100930-taskwizard](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100930-taskwizard.png)

Since we selected a specific event from the log, we are unable to edit the event to log.

![20100930-loggedevent](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100930-loggedevent.png)

The next thing we need to do is pick what action we want to trigger when our event shows up. There are three options; Start a Program, Send an email or Display a Message. The first time through we will choose, Start a Program.

![20100930-taskaction](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100930-taskaction.png)

Then you need to specify the options for your program. You can browse to the program you wish to run, I have selected the above script as my program. Since my script accepts an argument I set the argument of my script.

![20100930-scriptaction](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100930-scriptaction.png)

You can click through the remaining screens and your custom task is now set. The next step is to associate our second task to this event. The second task is to email the file that gets created as an attachment to yourself. Go through the first series of steps like we just did for the script, except we will choose Send an Email.

Fill in all the needed details for your email message and browse to the folder where your attachment is created.

![20100930-emailtask](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100930-emailtask.png)

Again click through the remaining screens to save our email task, and as soon as the event your are interested in appears you should have an email.
