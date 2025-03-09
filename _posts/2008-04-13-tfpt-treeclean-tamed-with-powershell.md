---
layout: post
permalink: http://blog.stangroome.com/2008/04/13/tfpt-treeclean-tamed-with-powershell/
title: TFPT TreeClean tamed with PowerShell
description: None
date: 2008-04-13 11:52:04 -0000
last_modified_at: 2010-07-18 22:27:12 -0000
publish: true
pin: false
categories:
- PowerShell
- TFS
tags: []
---
**Update:** [Philip Kelley](http://forums.microsoft.com/msdn/User/Profile.aspx?UserID=833470&SiteID=1) from Microsoft, [creator of TFPT](https://blogs.msdn.com/buckh/archive/2007/01/10/five-things-about-me.aspx), has kindly informed me that the July 2008 release of the TFS Power Tools is [now available for download](http://www.microsoft.com/downloads/details.aspx?FamilyID=00803636-1d16-4df1-8a3d-ef1ad4f4bbab&displaylang=en). This new version includes enhancements to TFPT TreeClean that allow you to specify which files to include or exclude and as such solves the main problem my TreeClean PowerShell script was created for. The output format of the new TreeClean also renders this script incompatible but the general concepts used by the script may still be useful.

* * *

I like the Team Foundation Server 2008 Power Tools, there are some great additions in there. One particular utility, TreeClean, has a great concept but is a little overzealous for my tastes. The purpose of TreeClean is to find all local files in your workspace folders that do not exist in source control and then allow you to delete all of them. The problem is that it includes *.user files in its find results and the delete option is all or nothing. The list of files can also be rather overwhelming. Thankfully we can get some more control by piping the results through PowerShell, starting with a simple script like this:

$ProgFiles = $Env:ProgramFiles ; $ProgFiles32 = (Get-Item "Env:ProgramFiles(x86)").Value ; if (-not [String]::IsNullOrEmpty($ProgFiles32)) { $ProgFiles = $ProgFiles32 ; }

$TFPTEXE = Join-Path -Path $ProgFiles ` -ChildPath "Microsoft Team Foundation Server 2008 Power Tools\TFPT.exe" ; if (-not (Test-Path -Path $TFPTEXE)) { throw "TFPT.EXE not found." ; }

[string]$Root = Resolve-Path -Path (Get-Location) ;

& $TFPTEXE treeclean ` | Where-Object { $_ -like ($Root + "*") } ` | Get-Item -Force ;

Once we have this script saved we can get more information from the results. For example, we can get count and list rogue files by extension:

TreeClean.ps1 | group Extension

We can exclude directories:

TreeClean.ps1 | ? { -not $_.PSIsContainer }

And finally we can delete everything but *.user files:

TreeClean.ps1 | ? { $_.Extension -ne ".user" } | Remove-Item

Now I can clean all the junk from my workspace but keep all my user-level project settings. However, while sorting through the extension-grouped report, looking for files to check-in before cleaning, there was a lot of noise from the build outputs. My quick solution:

gci -inc *.sln -rec | % { MSBuild /t:Clean $_ }

It also has the nice side effect of significantly reducing your workspace folder size if you want to zip it up and send it somewhere.
