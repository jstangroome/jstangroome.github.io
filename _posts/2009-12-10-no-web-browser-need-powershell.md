---
layout: post
permalink: http://blog.stangroome.com/2009/12/10/no-web-browser-need-powershell/
title: No Web Browser, Need PowerShell
description: None
date: 2009-12-10 07:48:23 -0000
last_modified_at: 2010-08-05 03:55:06 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
I recently found myself on a Windows Server 2003 machine without a functioning web browser and PowerShell wasn’t installed either. No problem, I just opened notepad and started typing:
  
    echo class Program { public static void Main() { >"%~dpn0.cs"
    echo using (var wc = new System.Net.WebClient()) { >>"%~dpn0.cs"
    echo wc.UseDefaultCredentials = true; >>"%~dpn0.cs"
    echo wc.DownloadFile(@"http://download.microsoft.com/download/1/1/7/117FB25C-BB2D-41E1-B01E-0FEB0BC72C30/WindowsServer2003-KB968930-x86-ENG.exe", @"%~dpn0.installer.exe");}}} >>"%~dpn0.cs"
    "%systemroot%\microsoft.net\framework\v3.5\csc.exe" /out:"%~dpn0.exe" "%~dpn0.cs"
    "%~dpn0.exe"
    "%~dpn0.installer.exe"

Saved it as a command script and double-clicked it. PowerShell v2 installer downloads and runs, bam! This would require tweaking for other processor architectures, other operating system versions, or older .NET installations. Thanks to [@tathamoddie](http://twitter.com/tathamoddie) for the proxy-friendly fix.
