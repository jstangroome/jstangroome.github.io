---
layout: post
permalink: http://blog.stangroome.com/2009/07/14/generic-batch-file-wrapper-for-powershell-scripts/
title: "Generic batch file wrapper for PowerShell\_scripts"
description: None
date: 2009-07-14 06:22:08 -0000
last_modified_at: 2010-07-18 08:32:22 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
Integrating PowerShell scripts into older style processes that are only designed to call out to executables or batch files (or “command scripts” as they have been known since NT4) can be slightly messy primarily due to the argument parsing semantics around double-quotes and file paths with spaces. I often end up writing a simple command script to wrap each PowerShell script to simplify this but it is tedious and repetitive and I’ve finally decided to create a generic script that works for any PowerShell script. Simply save the following code as a text file and save it with the same name as your PowerShell script but replace the .ps1 extension with a .cmd extension.
  
    @echo off
    setlocal
    set tempscript=%temp%\%~n0.%random%.ps1
    echo $ErrorActionPreference="Stop" >"%tempscript%"
    echo ^& "%~dpn0.ps1" %* >>"%tempscript%"
    powershell.exe -command "& \"%tempscript%\""
    set errlvl=%ERRORLEVEL%
    del "%tempscript%"
    exit /b %errlvl%

Now if your script is called “Get-Something.ps1”, you can simply run it like this:
  
    Get-Something “c:\some path\some.file” –SecondArg 3.14
