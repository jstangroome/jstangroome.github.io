---
layout: post
permalink: http://blog.stangroome.com/2007/06/20/powercleaning/
title: PowerCleaning
description: None
date: 2007-06-20 12:12:11 -0000
last_modified_at: 2010-07-18 22:37:06 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
I've often wanted to easily delete all old files below a folder that haven't been modified for a while. I've written a very small PowerShell function to do just that. By default it emulates the Disk Cleanup tool in Windows and deletes all files older than one week from the environment temp folder. function Remove-TemporaryItems ([string] $path = ($env:TEMP), [TimeSpan] $age = (New-Object System.TimeSpan 7, 0, 0, 0) ) { $files = Get-ChildItem $env:TEMP -recurse | ` Where-Object { ! $_.PSIsContainer -and $_.LastWriteTime.Add($age) -le [DateTime]::Now }; $files | Remove-Item; $files; }
