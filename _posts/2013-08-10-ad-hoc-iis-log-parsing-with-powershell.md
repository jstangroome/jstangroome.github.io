---
layout: post
permalink: http://blog.stangroome.com/2013/08/10/ad-hoc-iis-log-parsing-with-powershell/
title: Ad hoc IIS log parsing with PowerShell
description: None
date: 2013-08-10 02:30:57 -0000
last_modified_at: 2013-08-10 03:33:55 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
There are numerous log analysis systems for IIS and for log files in general and it is probably a good idea to use such a system for ongoing monitoring. However, sometimes you just have a bunch of IIS logs and some simple questions you want to ask of the data within. PowerShell is built into the OS so its an easy default choice for this scenario. Unfortunately the IIS W3C Extended log format doesn't quite align with PowerShell's built-in cmdlets. The [Import-Csv](http://technet.microsoft.com/en-us/library/hh849891.aspx) or [ConvertFrom-Csv](http://technet.microsoft.com/en-us/library/hh849890.aspx) cmdlets could come close when used with the -Delimiter parameter and some Header wrangling. You can even achieve a fair amount just by using [Get-Content](http://technet.microsoft.com/en-us/library/hh849787.aspx) and the [-split operator](http://technet.microsoft.com/en-us/library/hh847811.aspx) if you don't mind using array indexes to access different columns. With a little bit of extra regex and hashtable work, wrapped in a function for readability, the logs can be parsed quite simply and even handle the situation where the set of included columns changes half way through one of the log files. I wrote such a function in about 5 minutes the other day, called it "ConvertFrom-IISW3CLog" and put it on GitHub as [a Gist for future reference here](https://gist.github.com/6189660). It won't handle malformed log files but the point was to keep it simple, and not to re-invent another log system. You can use the function like this:
  
        gci c:\inetpub\logs\LogFiles\W3SVC1 | ConvertFrom-IISW3CLog

If you wanted to find how often each URL is accessed, a simple [Group-Object](http://technet.microsoft.com/en-us/library/hh849907.aspx) on the end of the pipe would tell you:
  
        gci c:\inetpub\logs\LogFiles\W3SVC1 | ConvertFrom-IISW3CLog | group cs-uri-stem

If you want to to find which URL requests have had errors today:
  
        $reqs = gci c:\inetpub\logs\LogFiles\W3SVC1 | ConvertFrom-IISW3CLog
    $reqs | ? { $_.'sc-status' -eq 500 -and $_.date -eq '2013-08-10' }

Once your log parsing gets more elaborate, you might want to look at the [Microsoft Log Parser](http://technet.microsoft.com/en-us/scriptcenter/dd919274.aspx) as a more capable solution.
