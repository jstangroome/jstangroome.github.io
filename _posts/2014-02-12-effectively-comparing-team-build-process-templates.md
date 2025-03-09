---
layout: post
permalink: http://blog.stangroome.com/2014/02/12/effectively-comparing-team-build-process-templates/
title: Effectively comparing Team Build Process Templates
description: None
date: 2014-02-12 07:52:50 -0000
last_modified_at: 2014-02-12 07:52:50 -0000
publish: true
pin: false
categories:
- PowerShell
- TFS
tags: []
---
I always prefer [implementing .NET build customizations through MSBuild](http://blog.stangroome.com/2012/02/08/rules-to-customising-a-dot-net-build/ "Rules to Customising a .NET Build") and I avoid modifying the Windows Workflow XAML files used by Team Build. However, some customizations are best implemented in the Team Build process, like chaining builds to execute in succession and pass information between them. As a consultant specializing in automated build an deployment I also spend a lot of time understanding Workflow customizations implemented by others. For me the easiest way to understand the customizations implemented in a particular Team Build XAML file is to use a file differencing tool to compare the current workflow to a previous version of the workflow, or even to compare it to the default Team Build template it was based on. Unfortunately, the Windows Workflow designer in Visual Studio litters the XAML file with a lot of view state, obscuring the intended changes to the build process amongst irrelevant designer-implementation concerns. To address this problem, I wrote [a PowerShell script (available as a GitHub Gist)](https://gist.github.com/jstangroome/8951543) which removes all the elements and attributes from the XAML file which are known to be unimportant to the process it describes. Conveniently, the XAML file itself lists the set of XML namespace prefixes that can be safely removed in an [mc:Ignorable attribute](http://msdn.microsoft.com/en-us/library/aa350024.aspx) on the root document element. Typically I use my XAML cleaning PowerShell script before each check-in to ensure the source control history stays clean but I have also used it on existing XAML files created by others to canonicalize them before opening them in a diff tool. Using the script is as simple as:
  
        .\Remove-IgnoreableXaml.ps1 -Path YourBuildTemplate.xaml

  Or, if you don't want to overwrite the file in place, specify an alternate destination:
  
        .\Remove-IgnoreableXaml.ps1 -Path YourBuildTemplate.xaml -Destination YourCleanBuildTemplate.xaml

 
