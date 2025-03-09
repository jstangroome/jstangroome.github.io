---
layout: post
permalink: http://blog.stangroome.com/2012/04/11/beware-of-unqualified-powershell-type-literals/
title: Beware of unqualified PowerShell type literals
description: None
date: 2012-04-11 02:11:45 -0000
last_modified_at: 2012-04-11 02:11:45 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
In PowerShell we can refer to a type using a type literal, eg:
  
        [System.DateTime]

Type literals are used when casting one type to another, eg:
  
        [System.DateTime]'2012-04-11'

Or when acessing a static member, eg:
  
        [System.DateTime]::UtcNow

Or declaring a parameter type in a function, eg:
  
        function Add-OneWeek ([System.DateTime]$StartDate) {
        $StartDate.AddDays(7)
    }

PowerShell also provides a handful of type accelerators so we don't have to use the full name of the type, eg:
  
        [datetime] # accelerator for [System.DateTime]
    [wmi] # accelerator for [System.Management.ManagementObject]

However, unlike a C# project in Visual Studio, PowerShell will let you load two identically named types from two different assemblies into the session:
  
        Add-Type -AssemblyName 'Microsoft.TeamFoundation.Client, Version=**10**.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'
    Add-Type -AssemblyName 'Microsoft.TeamFoundation.Client, Version=**11**.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

Both of the assemblies in my example contain a class named TfsConnection, so which version of the type is referenced by this type literal?
  
        [Microsoft.TeamFoundation.Client.TfsConnection]

From my testing it appears to resolve to the type found in whichever assembly was loaded first, in my example above it would be version 10. However, a single script or module would be unlikely to load both versions of the assembly itself so you would likely encounter this situation when two different scripts or modules load conflicting versions of the same assembly, and in this case you won't control the order in which each assembly is loaded so you can't be sure which is first. It is possible for a script to detect if a conflicting assembly version is loaded if it is expecting this scenario but the CLR won't allow an assembly to be unloaded so all the script could do is inform the user and abort, or maybe spawn a child PowerShell process in which to execute. It is also possible to have two identically named types loaded in PowerShell via another less obvious scenario. If you use the [New-WebServiceProxy cmdlet](http://technet.microsoft.com/en-us/library/hh849841.aspx) against two different endpoints implementing the same web service interface, PowerShell generates and loads two different dynamic assemblies with identical type names (assuming you specify the same Namespace parameter to the cmdlet). I've run into this issue with the SQL Server Reporting Services web service. To address this issue, referring to my first example, you can use assembly-qualified type literals, eg:
  
        [Microsoft.TeamFoundation.Client.TfsConnection, Microsoft.TeamFoundation.Client, Version=11.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a]

However these quickly create scripts which are harder to read and maintain. For accessing static members you can assign the type literal to a variable, eg:
  
        $TfsConnection11 = [Microsoft.TeamFoundation.Client.TfsConnection, Microsoft.TeamFoundation.Client, Version=11.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a]
    $TfsConnection11::ApplicationName

But for casting and declaring parameter types I don't have a better solution. There are ways to add to PowerShell's built-in type accelerator list but it involves manipulating non-public types which I wouldn't feel comfortable using in a script or module I intend to publish for others to use. For the New-WebServiceProxy situation, I have [created a wrapper function](https://github.com/codeassassin/PSThycoticSecretServer/blob/master/src/New-NamedWebServiceProxy.ps1) which will reuse the existing PowerShell generated assembly if it exists.
