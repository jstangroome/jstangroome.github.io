---
layout: post
permalink: http://blog.stangroome.com/2007/09/06/my-manwich-powershell-which/
title: My Manwich! PowerShell Which
description: None
date: 2007-09-05 22:22:20 -0000
last_modified_at: 2013-07-25 11:07:53 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
One feature I've loved during my brief ventures into Linux-land is the [which](http://www.linuxmanpages.com/man1/which.1.php) command, not to be confused with other [tasty items](http://en.wikipedia.org/wiki/Manwich). For those not familiar with the command, it enables you to determine the full path to an executable in your environment path that would be executed if only the name of the executable is entered at the command prompt. This is useful in situations where you have different versions of the same executable on your file system (perhaps even a malicious version) or if you simply want to know where the executable resides. I have often wanted to perform a similar search on my Windows PCs but to the best of my knowledge, the standard command prompt does not provide an easy way to do this. Gratefully, as is becoming the pattern lately, PowerShell makes this very simple. Simple to the point that I don't really need to bundle it into a function. For example, here is how I locate the SQL Server Management Studio executable that runs when I use Start, Run, "sqlwb":
  
    $Env:Path.Split(';') | Get-ChildItem -Filter sqlwb*

If you don't have SQL installed, you can switch the -filter parameter for "calc*" or similar. You can see that it isn't limited to just executable files on your path either and while it doesn't duplicate the default behaviour of the Linux which command, wrapping the one-liner into a function with some extra switches would get it very close. One additional scenario to support in a full implementation of the which command would be the the list of application paths in the registry here:
  
    Get-ChildItem -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths'
