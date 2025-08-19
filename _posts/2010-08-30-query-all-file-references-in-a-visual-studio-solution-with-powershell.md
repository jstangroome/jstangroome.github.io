---
permalink: /2010/08/30/query-all-file-references-in-a-visual-studio-solution-with-powershell/
title: Query all file references in a Visual Studio solution with PowerShell
date: 2010-08-30 09:52:06 -0000
last_modified_at: 2010-08-30 09:52:06 -0000
publish: true
pin: false
categories:
- PowerShell
- Visual Studio
tags: []
---
Today I was working on introducing Continuous Integration to a legacy code base and was discovering the hard way that the solution of about 20 projects had many conflicting references to external assemblies. Some assemblies were different versions, others the same version but in different paths, and others completely missing altogether. Needless to say this wasn't going to build cleanly on a build server. Rather than manually checking the path and version of every assembly referenced by every project in the solution, I wrote a PowerShell script to parse a Visual Studio 2010 solution file, identify all the projects, then parse the project files for the reference information. The resulting [Get-VSSolutionReferences.ps1 script is available on GitHub](http://gist.github.com/557222). Once I had a collection of objects representing all the assembly references I could perform some interesting analysis. Here is a really basic example of how to list all the assemblies referenced by two or more projects:
  
    $Refs = & .\Get-VSSolutionReferences.ps1 MySolution.sln
    $Refs | group Name | ? { $_.Count -ge 2 }

It doesn't take much more to find version mismatches or files that don't exist at the specified paths, but I'll leave that as an exercise for you, the reader.
