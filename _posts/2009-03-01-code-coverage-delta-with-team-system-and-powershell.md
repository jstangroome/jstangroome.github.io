---
layout: post
permalink: http://blog.stangroome.com/2009/03/01/code-coverage-delta-with-team-system-and-powershell/
title: Code Coverage Delta with Team System and PowerShell
description: None
date: 2009-03-01 11:37:09 -0000
last_modified_at: 2010-07-18 09:51:07 -0000
publish: true
pin: false
categories:
- PowerShell
- TFS
tags: []
---
My last post documented my first venture into working with Visual Studio's Code Analysis output with PowerShell to find classes that need more testing. Since then I've taken the idea further to analyse how coverage has changed over a series of builds from TFS. What resulted is a PowerShell script that takes, as a minimum, the name of a TFS Project and the name of a Build Definition within that project. The script will then, by default, locate the two most recent successful builds, grab the code coverage data from the test runs of each of those builds and output a list of classes whose coverage has changed between those builds, citing the change in the number of blocks not covered. Additional parameters to the script allow partially successful or failed builds to be considered and also to analyse coverage change over a span of several builds rather just two consecutive builds. The primary motivator behind developing this script was to be able to identify more accurately where coverage was lost when a new build has an overall coverage percentage lower than the last. This then helps to locate, among other things, where new code has been added without testing or where existing tests have been deleted or disabled. A code base with a strong commitment to the Single Responsibility Principle should find class-level granularity sufficient but extending the script to support method-level reporting should be trivial given that the Coverage Analysis DataSet already includes all the required information. The script requires the Team Foundation Server PowerShell Snapin from the TFS October 2008 Power Tools and the Visual Studio Coverage Analysis assemblies presumably available in Visual Studio 2008 Professional or higher. These dependencies only support 32-bit PowerShell so my script unfortunately suffers the same constraint. [Download the script here](http://www.codeassassin.com/public/Compare-TfsBuildCoverage.zip), and use it something like this: PS C:\TfsWorkspace\> C:\Scripts\Compare-TfsBuildCoverage.ps1 -project Foo -build FooBuild | sort DeltaBlocksNotCovered
