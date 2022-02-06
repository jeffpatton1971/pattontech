---
layout: post
title:  "Exporting Event logs in the normal Event Log format"
date:   2012-04-01 15:18:00 -0600
categories: blog
tags: April 2012 KU SOECS Scripting
---
I’ve decided that I’d like to be able to export my event logs in their native .evtx file format. This appears to be faster than converting them all to .csv files. Early on I ran into a few problems, the first of which I was unable to convert what was in my head to something that Google understood! Once I got over that I found what I was looking for.

[System.Diagnostics.Eventing](http://msdn.microsoft.com/en-us/library/system.diagnostics.eventing.aspx)

[System.Diagnostics.Eventing.Reader](http://msdn.microsoft.com/en-us/library/system.diagnostics.eventing.reader.aspx)

For the purposes my function, what I’m looking for is found within the Reader namespace. I’d like my function to have a similar look and feel to the built-in cmdlet’s, like Get-WinEvent. So the first thing I decided I would do is implement a –ListLog switch parameter.

This parameter will call the [GetLogNames()](http://msdn.microsoft.com/en-us/library/system.diagnostics.eventing.reader.eventlogsession.getlognames.aspx) method of the [EventLogSession](http://msdn.microsoft.com/en-us/library/system.diagnostics.eventing.reader.eventlogsession.aspx) Class. So the first thing you need to do is create a new sessions.

``` powershell
$EventSession = New-Object System.Diagnostics.Eventing.Reader.EventLogSession
```

Once we’ve done that we simply call the GetLogNames() method from our new object and a list of logs will appear

``` powershell
$EventSession.GetLogNames()

Application
HardwareEvents
Internet Explorer
Key Management Service
Media Center
OAlerts
Operations Manager
Security
System
Windows PowerShell
```

The next thing I need to be able to do is the actual exporting of the logs. There are actually two methods exposed in the EventLogSession class. The first is [ExportLog()](http://msdn.microsoft.com/en-us/library/bb302895.aspx) and the second is [ExportLogAndMessages()](http://msdn.microsoft.com/en-us/library/bb361235.aspx). The documentation states that the difference between the two is the latter exports the log and it’s messages. To be safe, I’ll use the latter, ExportLogAndMessages() which will grab that metadata.

This is where I ran into the first hiccup. The breakdown of each of those is as follows

* Path | LogName as String
* PathType as PathType
* Query as String
* targetFilePath as String

Now, most of the examples I found online appeared to use PathType as an object. The problem is it really isn’t, it’s a string that contains either the word ‘LogName’ or ‘FilePath’. Technically that really isn’t even a problem, it seems to me to be more of a documentation issue. But it could also be poor understanding on my part, at any rate, there are several ways to deal with this and I chose the easy one.

Since I’m going to assume that you want to export an actual EventLog and not a file, for obvious reasons, then I’m only going to give you the option of LogName. This makes exporting your log look something like this.

``` powershell
$EventSession.ExportLogAndMessages($LogName,’LogName’,’*’,$Destination)
```

Now I could have made it look much more complicated by changing ‘LogName’ to something like this

``` powershell
$EventSession.ExportLogAndMessages($LogName,[System.Diagnostics.Eventing.Reader]::LogName,’*’,$Destination)
```

But that just seemed to me to be too much.

I’m ignoring the Query option first and focusing on targetFilePath. In testing, this works beautifully you pass in the full path and filename of the file to be created, and it appears. Now when I started testing this against remote machines I ran into my second problem.

When I create my session against a remote computer

``` powershell
$ComputerName = ‘ServerA’
$EventSession = New-Object System.Diagnostics.Eventing.Reader.EventLogSession($ComputerName)
```

I can get the proper list of logs, but when I ran the ExportLogAndMessages() method, I didn’t see the exported logfile in my folder. Turns out you need to be aware of the context, if you are connecting to a remote machine you need to remember that everything get’s executed on that remote machine. That means that when the following code is executed

``` powershell
$Destination = ‘C:LogFilesApplication.evtx’
$EventSession.ExportLogAndMessages($LogName,’LogName’,’*’,$Destination)
```

That file actually exists on the remote filesystem (ServerA) and not the local disk. At the moment I’ve not decided how I want to handle this, or if I even want to bother. You see when I attempt to trick the method and provide a UNC I get the following

``` powershell
$EventSession.ExportLogAndMessages(‘Application’,’LogName’,’*’,’\pc01C$LogFilesapp.evtx’)

Exception calling “ExportLogAndMessages” with “4” argument(s): “Attempted to perform an unauthorized operation.”
At line:1 char:29
+ $EventSession.ExportLogAndMessages <<<< ('Application','LogName','*','\pc01C$LogFilesapp.evtx')
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : DotNetMethodException
```

My next obstacle was credentials. Remote machines may require a different user/pass combination than what you’re current login context might be. Fortunately I can pass that information into the class. One of the [constructors](http://msdn.microsoft.com/en-us/library/bb352836.aspx) has 5 properties

* ComputerName as string
* Domain as string
* Username as string
* Password as SecureString
* LogonType as SessionAuthentication

Since I store my own admin credentials locally in a file I know I have access to most of that information right from the console. The first two examples will display my logon domain and username.

``` powershell
$Credential.GetNetworkCredential().Domain
$Credential.GetNetworkCredential().Username
```

The next one is a little scary, but if you think about it, it’s not as bad as you think it might be. First off, running this command will display my unencrypted password on the console! The HORROR! It’s really ok, the reason that works is because I set it in my context, so I have access to it. Get it? It’s ok if you don’t, it took me a while to figure that out as well, it’s encrypted in memory so while I can view it clear text, another use on the same system shouldn’t be able to.

``` powershell
$Credential.GetNetworkCredential().Password
```

The only problem with the previous is the outputted password is a string. The constructor needs this as a SecureString. Fortunately the following command is just that.

``` powershell
$Credential.Password
```

Now, I’m by no means an expert on .Net. I’m not even sure I would say I’m knowledgeable, but I certainly know enough to be extremely dangerous. As I was looking at the page that listed how to connect remotely I noted that LogonType was worded in a similar fashion as was PathType, so before I got carried away I decided to try each of the 4 [LogonTypes](http://msdn.microsoft.com/en-us/library/system.diagnostics.eventing.reader.sessionauthentication.aspx).

* Default
* Negotiate
* Kerberos
* NTLM

In my testing against a remote machine that my current user context had no rights on, my admin credentials worked for each of the various types. So, as far as I’m concerned that seems to work, so I decided to stick with Default.

So now I’m able to connect to a local or remote machine and export out the logs to an existing folder on the hard drive. That leaves one final problem to deal with, handling a folder that doesn’t exist yet. I leave it up to the user to pass in the folder and filename to write to. So if it doesn’t exist I need to make it. I had thought about splitting the Destination variable into two, FilePath and FileName, but decided I didn’t want to do that.

Since I’m treading the deep waters of .Net I decided that since my Destination looks like a legitimate path, it may behave like one. I started browsing the [System.IO](http://msdn.microsoft.com/en-us/library/system.io.aspx) namespace and originally was looking at File, and then realized I was dealing with a directory, which made things much easier.

I know that there is a parent property when you grab a path using [Get-ChildItem](http://technet.microsoft.com/en-us/library/dd347686.aspx) so I figured there ought to be something similar in [System.IO.Directory](http://msdn.microsoft.com/en-us/library/system.io.directory.aspx). Turns out it’s more or less exactly the same thing.

I kind of have this phobia about tweaking data that is passed into my scripts, so while this looks ugly, I’m really quite pleased.

```powershell
([System.IO.Directory]::GetParent($Destination)).FullName
```

What does this do? Well assuming that Destination is C:LogFiles, that code returns C:LogFiles, but if it happens to be, C:LogFilesPathToRealyDeepFolder it returns everything above Folder. Which works out quite nicely. I’m assuming that the tail end will be a filename, so I ask .Net for the parent path of the filename and then create that path.

Locally creating this was simple, but we run into issues again remotely. While [New-Item](http://technet.microsoft.com/en-us/library/dd347566.aspx) has a Credential property, the underlying file system doesn’t support that. So instead of getting crazy I decided to use a ScriptBlock and the [Invoke-Command](http://technet.microsoft.com/en-us/library/dd347578.aspx) cmdlet.

Since we are passing variables to a remote machine, by default ServerA won’t know what Destination represents, so we use the Argumentlist property of Invoke-Command.

$ScriptBlock = {New-Item -Path $args[0] -ItemType Directory -Force}

Invoke-Command -ScriptBlock $ScriptBlock -ComputerName $ComputerName -Credential $Credential -ArgumentList (([System.IO.Directory]::GetParent($Destination)).FullName) |Out-Null

As you can see in my ScriptBlock, args[0] is represents the Path we need to create. In order for that to make it over to the remote machine you will see in the Invoke-Command line I pass in my corrected Destination as an ArgumentList.

The result is a working [Export-EventLogs](https://github.com/mod-posh/ComputerManagement#export-eventlog) function that will actually export the log in the native format. It was a lot of work to get this all together, but I think it will be very useful. I decided against any sort of clearing function since there is already a built-in for that, but I haven’t seen a built-in for exporting the logs.

This function can also be downloaded from my [TechNet Gallery](http://gallery.technet.microsoft.com/Export-EventLog-18a87c2c)(Gallery is no longer active).
