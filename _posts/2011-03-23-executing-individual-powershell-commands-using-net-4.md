---
permalink: http://blog.stangroome.com/2011/03/23/executing-individual-powershell-commands-using-net-4/
title: Executing individual PowerShell 2 commands using .NET 4
description: None
date: 2011-03-23 09:34:27 -0000
last_modified_at: 2013-08-13 22:38:47 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
One of the many great things about PowerShell is that it can utilise the .NET framework directly and third party .NET libraries whenever PowerShell doesn't offer a native solution. However, the PowerShell console, the Integrated Scripting Environment (ISE) and PS-Remoting in PowerShell 2.0 are all built for use with .NET 2.0 through to .NET 3.5. With the release of version 4 of the .NET Framework though, there is an increasing amount of core functionality and third-party assemblies that are not accessible from PowerShell — the new [Is64BitOperatingSystem property](http://msdn.microsoft.com/en-us/library/system.environment.is64bitoperatingsystem.aspx) on the System.Environment class is a simple example. So, given that .NET 4 has excellent support for being able to run most .NET 2 assemblies but PowerShell doesn't have officially support for .NET 4 yet, how can we safely utilise new .NET 4 functionality from PowerShell without waiting for a new PowerShell release from Microsoft? A quick search of the web will reveal a few different approaches, each with their own major drawbacks:

* Change the system-wide registry setting to load all .NET 2 assemblies under .NET 4 instead for all .NET applications.
* Change the system-wide config files for the PowerShell console, ISE, and Remoting service to use .NET 4 instead for all PowerShell sessions.
* Build a small .NET 4 application to host a PowerShell Runspace in-process as .NET 4.

These options are either have too wide an effect or vary too much from the standard PowerShell experience for me to adopt. What if there was a way, from inside a standard PowerShell 2 session to execute a single command, script block, or script under .NET 4 without touching any upfront configuration, without requiring elevated permissions, and without affecting anything else on the system? This solution is available in the form of the new [Activation Configuration Files](http://msdn.microsoft.com/en-us/library/ff361644.aspx) feature also introduced in .NET Framework 4.0. By dynamically creating a small config file in a temporary directory then setting a process-scoped environment variable we can easily start a new .NET 4 PowerShell session passing in a ScriptBlock and some arguments and receive the same deserialized objects in return as you would see when using Remoting. I've implemented my first version of this technique as [a PowerShell script module available on Gist.GitHub](https://gist.github.com/882528). It includes an Invoke-CLR4PowerShellCommand Cmdlet which behaves just like a very simple version of the built-in Invoke-Command Cmdlet. The module also includes a Test-CLR4PowerShell function demonstrating basic use of the module.
