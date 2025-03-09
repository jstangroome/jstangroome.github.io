---
layout: post
permalink: http://blog.stangroome.com/2010/08/11/automatic-tfs-check-out-for-powershell-ise/
title: Automatic TFS Check Out for PowerShell ISE
description: None
date: 2010-08-11 08:13:56 -0000
last_modified_at: 2010-08-11 08:13:56 -0000
publish: true
pin: false
categories:
- PowerShell
- TFS
tags: []
---
I work with a lot of PowerShell scripts, it's the fun part of my job. I do most of my editing using the [PowerShell ISE](http://technet.microsoft.com/en-us/library/dd315244.aspx), primarily because it is [the default](http://www.useit.com/alertbox/defaults.html), but also because it has great syntax highlighting, tab completion, and debug support. Additionally, all of my scripts are in source control: sometimes [Mercurial](http://mercurial.selenic.com/), occasionally [Git](http://git-scm.com/), but mostly [Team Foundation Server](http://msdn.microsoft.com/en-us/vstudio/ff637362.aspx) (TFS). The way TFS works though, is that all files in your local workspace are marked as read-only until you Check-Out for editing. When working on TFS-managed files in Visual Studio, the check-out is done automatically as soon as you begin typing in the editor. The PowerShell ISE however has no idea about TFS and will let you edit to your heart's content until you eventually try to save your changes and it fails due to the read-only flag. But I've developed a solution... I've written a [PowerShell ISE-specific profile script](http://gist.github.com/518638) that performs a few simple things:

  1. Checks if you have the TFS client installed (eg Team Explorer).
  2. Registers for ISE events on each open file and any files you open later.
  3. Upon editing of a file, if it is TFS-managed then checks it out.

The end result is the same TFS workflow experience from within the PowerShell ISE as Visual Studio provides.
