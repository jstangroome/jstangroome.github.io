---
layout: post
permalink: http://blog.stangroome.com/2007/07/25/touched-by-powershell/
title: Touched By PowerShell
description: None
date: 2007-07-25 11:38:53 -0000
last_modified_at: 2010-07-18 22:36:08 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
Today I was asked how to "touch" all files in a folder in Windows to set the Modified timestamp to now. If I was asked about the Created timestamp I would have suggested to copy the files to a new folder and use the copies which will have a new Created timestamp. Unfortunately Windows doesn't seem to have an easy way to edit the Modified timestamp. Thankfully, everyone who has Windows also has PowerShell, or [should have](http://www.microsoft.com/windowsserver2003/technologies/management/powershell/download.mspx), or should [upgrade their OS](http://www.microsoft.com/windowsserver2008/powershell.mspx). With PowerShell, touching files is easy: Get-ChildItem * | % { $_.LastWriteTime = [DateTime]::Now }
