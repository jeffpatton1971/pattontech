---
layout: post
title:  "Windows Eventlog Management Part 2"
date:   2012-04-01 15:11:00 -0600
categories: blog
tags: April 2012 KU SOECS Scripting MissingImages
---
HOW TO GET THE LOG TO LET YOU KNOW WHEN SOMETHING HAPPENED

Event Triggers

* Specify a custom action when a particular event occurs
  * Start a program
  * Send an email
  * Display a message
* Use scripting to give yourself flexibility
* Be careful about email

Triggers are one of those really awesome things that you wish had been around in Windows from the beginning. The idea is that when a particular event occurs, you want to perform some action. You can start a program or script or send an email, those first two are perhaps the one’s you’ll use most.

For myself I find the Start Program option the best of the bunch, being a sysadmin I find myself routinely writing scripts to perform one or more things. If I’m interested in a particular event I can create a script that will give me additional information surrounding that event.

I have a few of these in place right now on my file server I have a trigger on Event ID 2013, the low disk message. The default message is rather cryptic, simply stating that a given disk is getting close to full. Fortunately it does give me a vital piece of information, the drive letter. So I have a script that pulls that entry from the log, grabs the disk letter, and queries WMI for the free space of the disk, the script stores that as an XML file that I have the Task email to me. So you can use a script to flesh out a rather vague entry.

On the opposite side of that coin, there are some events that you are interested in that happen so frequently that sending you an email each time they occur would be overwhelming. Going back to my example of the Print Server logs, I manage two print servers that I have divided between lab use and staff/faculty use. I have written up my own print logging script that generates a daily CSV of printer usage. With two servers, about 50 printers and over 3,000 users who can print to them you could imagine what my inbox would look like if I had that emailed to me at each print.

Creating an Event Trigger

* Find the event you want to be notified about
* Create a script that gives you more info
* Attach a task to the Event
* Choose an Action
* Configure the Action
* Set the context for the Task

Now that you are familiar with your logs, and have determined what specific log entry you want to know about, it’s time to do something about it. The example I will be using is from my DHCP server, I’d like to know when a computer asks for an IP and is denied because the MAC address is unknown to me.

I have written a script that gives me the MAC, Hostname, Message, and Time at which the client asked. Since a given client may potentially ask every 5 minutes until it gets a lease, I don’t want an email. In fact, since a given client can ask multiple times, I just want a file with the MAC address as part of it so I can, at a glance, get an idea of how many devices are trying to connect.

[Create a script]()

![FindEvent]()

There are actually two events that I’m interested in, this means that I’ll need my script to accept the Event ID as a parameter. Also, neither of these events are Error or Warning events, merely informational, letting me know a computer was unable to get an address.

[Create a script](https://github.com/jeffpatton1971/mod-posh/blob/master/powershell/production/Get-DHCPDenies.ps1)

![Get-DHCPDenies]()

I’m pretty good at writing scripts to get the information I need, but if you’re not comfortable scripting by all means you could run a command-line utility. There are quite a few available in the [Sysinternals suite](http://technet.microsoft.com/en-us/sysinternals/bb545021), not to mention some very handy [built-in tools](http://technet.microsoft.com/en-us/library/dd560674(v=WS.10).aspx) on Windows Server 2008. This script accepts the EventID and outputs an XML file named for the MAC that triggered the event.

Create the trigger

![task1]()

Give your task a name and a description.

Choose an action

![task2]()

Pick whether you need to start a program, send an email or display a message. The wizard allows you to only set one Action, but you should be aware that you can have as many as you want so pick one to start with and then mix and match later!

Configure your action

![task3]()

So if you’re using a script you need to specify the script interpreter to run. For this example I’m running a PowerShell script which is why I typed in powershell.exe. But it could just as easily have been Cscript, or Python, or the utility of your choice. If you’re running a script then the argument is the script itself along with any parameters you need to pass it. I keep all my scripts in the same place, so I define the Start In folder to be that location.

Set the context

![task4]()

You will notice that I have set this task to run whether or not someone is logged in. I have not stored a password with this account so it will run as the system. That’s something to keep in mind, if you’re uncomfortable doing this, you may want to create a service account to run as.

That’s it, after you click Ok, the trigger is done. All you need to do now is sit back and watch as those files get created.

Now that we have our triggers, let’s see how we can get a notification when something happens.

[Part 1](2012-04-01-windows-eventlog-management-part1.md)

Part 2
