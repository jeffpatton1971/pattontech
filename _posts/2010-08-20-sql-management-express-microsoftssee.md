---
layout: post
title:  "SQL Management Express and Microsoft##SSEE"
date:   2010-08-20 19:21:00 -0600
categories: blog
tags: August 2010 KU SOECS
comments: false
---
Download and install [Microsoft SQL Server Management Studio Express](http://www.microsoft.com/downloads/en/results.aspx?freetext=Microsoft+SQL+Server+Management+Studio+Express&displaylang=en&stype=s_basic)

Download and install [Microsoft SQL Server 2005 Express Edition Toolkit](http://www.microsoft.com/downloads/en/results.aspx?freetext=Microsoft+SQL+Server+2005+Express+Edition+Toolkit&displaylang=en&stype=s_basic)

Configure the built-in SQL instance for remote management by SQL Server Management Studio Express

1. Open the “SQL server Configuration Manager”
2. Right-click the line “Protocols for MICROSOFT##SSEE” and choose “Properties”
3. Set “hide instance” field is set to “no” instead
4. Under protocols, enable TCP/IP and Named Pipes
5. Restart the microsoft##ssee service

I found the above steps [here](http://channel9.msdn.com/forums/TechOff/255490-How-to-connect-MICROSOFTSSEE/).

Access the SQL instance using SQL Server Management Studio Express

1. Start SQL Server Management Studio Express
2. Connect to this instance: \.pipeMSSQL$MICROSOFT##SSEEsqlquery
3. You may need to check Windows Authentication

I found the instance name [here](http://channel9.msdn.com/forums/TechOff/255490-How-to-connect-MICROSOFTSSEE/?CommentID=463066).

Now that you have access to the instance you may perform regular tasks to it, such as backing it up, detaching it and so on.
