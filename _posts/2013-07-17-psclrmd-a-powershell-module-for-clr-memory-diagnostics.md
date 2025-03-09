---
layout: post
permalink: http://blog.stangroome.com/2013/07/17/psclrmd-a-powershell-module-for-clr-memory-diagnostics/
title: PSClrMD - A PowerShell module for CLR Memory Diagnostics
description: None
date: 2013-07-17 07:05:43 -0000
last_modified_at: 2013-07-18 03:17:36 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
Back in May, [the .NET Framework team blogged](http://blogs.msdn.com/b/dotnet/archive/2013/05/01/net-crash-dump-and-live-process-inspection.aspx) about a new set of advanced APIs for programmatically inspecting a live process or crash dump of a .NET application. These APIs are called "CLR Memory Diagnostics" or "ClrMD" for short and are [available as a pre-release NuGet package](http://nuget.org/packages/Microsoft.Diagnostics.Runtime) called "Microsoft.Diagnostics.Runtime" - I think there may be some naming issues yet to be resolved. Based on some of the examples on their blog post demonstrating a LINQ-style approach I thought this library would be very useful in a PowerShell pipeline scenario as well. Although there is already a [PowerShell module for debugging with WinDbg](https://powerdbg.codeplex.com/) (PowerDbg), I wanted the practice of building a PowerShell module and the opportunity to play with the ClrMD library. Today I started building the first set of cmdlets based on the examples demonstrated in the blog's code samples and have [published the code on GitHub](https://github.com/codeassassin/PSClrMD). The cmdlets so far are:
    * **Connect-ClrMDTarget** \- establishes the underlying DataTarget object by attaching to a live process or loading a crash dump file.
    * **Get-ClrMDClrVersion** \- lists the versions of the CLR loaded in the connected process. Typically just one.
    * **Connect-ClrMDRuntime** \- establishes the underlying ClrRuntime object to query .NET-related information. Defaults to the first loaded CLR version in the process.
    * **Get-ClrMDThread** \- lists the CLR threads of the connected CLR runtime.
    * **Get-ClrMDHeapObject** \- lists all the objects in the heap of the connected CLR runtime.
    * **Disconnect-ClrMDTarget** \- detaches from the connected process.
The ClrMD API is centered around having a DataTarget and ClrRuntime instance as context for performing all other operations. In PowerShell, it would be awkward to pass this context as a parameter to every cmdlet so I wrote the Connect cmdlets to store the context in a module variable which all other cmdlets will naturally inherit. If desired however, the Connect cmdlets accept a **-PassThru** switch which will output a context object which can then be passed explicitly to the -**Target** or **-Runtime** parameters of the other cmdlets. This would enable two or more processes to be inspected simulataneously, for example. Included in the source repository is a [drive.ps1](https://github.com/codeassassin/PSClrMD/blob/master/PSClrMD/drive.ps1) script which I used during development to repeatedly try different scenarios and set some default formatting for threads and heap objects. One example in this script is finding the first 20 unique string values in the process, here is an excerpt:
  
        Import-Module -Name PSClrMD
    Connect-ClrMDTarget -ProcessId $PID
    Connect-ClrMDRuntime
    Get-ClrMDHeapObject | 
     Where-Object { $_.IsString } | 
     Select-Object -Unique -First 20 -Property SimpleValue
    Disconnect-ClrMDTarget

From this you can hopefully see how easy it can be to connect to a running process (PowerShell itself in this case) and query interesting data. From this it also appears that I should try to combine the two Connect cmdlets into one for the common scenario demonstrated here. Another example to be found in the drive.ps1 script is listing the top memory-consuming objects by type which, when combined with scheduled tasks and Export-Csv, could provide a simple monitoring solution. You can [download the compiled module here](https://github.com/codeassassin/PSClrMD/releases/tag/v0.7.1) or feel free to get the source and submit a Pull Request for anything you add - I've only scratched the surface of what ClrMD exposes. **Update:** there is also a [scriptcs](http://scriptcs.net/) [Script Pack for ClrMD](http://blog.hackedbrain.com/2013/05/18/scriptcs-clrmd-enabling-rich-programmatic-net-diagnostics/) if PowerShell is not your style.
