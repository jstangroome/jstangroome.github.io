---
layout: post
permalink: http://blog.stangroome.com/2012/04/13/adopting-psget-as-my-preferred-powershell-module-manager/
title: Adopting PsGet as my preferred PowerShell module manager
description: None
date: 2012-04-13 06:24:30 -0000
last_modified_at: 2012-04-13 06:24:30 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
I recently blogged about[what features I think a PowerShell module manager should have](http://blog.codeassassin.com/2012/02/23/requirements-for-a-powershell-module-manager/) and I briefly mentioned a few existing solutions and my current thoughts about them. A few people left comments mentioning some other options (which I've now looked into). I finished the post suggesting that it would be better to for me to contribute to one of the existing solutions than to introduce my own new alternative to the mix. So I did. I chose [Mike Chaliy](https://twitter.com/chaliy)'s [PsGet](http://psget.net/) as my preferred solution (not to be confused with Andrew Nurse's PS-Get) because I liked its current feature set, implementation, and design goals most. I have been committing to PsGet regularly, implementing the missing features I blogged about previously and anything else I've discovered as part of using it for my day job. My favourite contributions that I have made to PsGet so far include:
    * Support for non-default values of the PSModulePath environment variable and for [installing modules (and PsGet itself) to any path](https://github.com/psget/psget/wiki/How-PsGet-decides-where-to-install-modules), ignoring the value of PSModulePath.
    * Allow a hash (SHA256) of the module to be passed to Install-Module to [ensure only trusted modules get installed](https://github.com/psget/psget/wiki/How-to-ensure-the-correct-module-is-installed) as an alternative to Execution Policy and files marked with Internet Zone.
    * Support for modules hosted on the [public NuGet repository](http://nuget.org/packages?q=powershell).
Since I adopted PsGet, [Ethan Brown](https://github.com/Iristyle) also contributed some important changes to support modules hosted on CodePlex, the [PowerShell Community Extensions](http://pscx.codeplex.com/) being a popular example. I've also been [experimenting](https://github.com/codeassassin/psget/tree/ScriptExplorer) with integrating PsGet with the same web service that Microsoft's new [Script Explorer](http://blogs.msdn.com/b/powershell/archive/2012/04/09/microsoft-script-explorer-for-windows-powershell-beta-1-now-available.aspx) uses to download the wide selection of scripts and modules available from the[TechNet Script Repository](http://gallery.technet.microsoft.com/ScriptCenter/). PsGet also handles the scripts hosted on [PoshCode](http://poshcode.org/) well but I think we can improve the search capability. Looking back at the list of requirements I made for a PowerShell module manager, this is what I think remains for PsGet, in order of most important first:
    1. Side-by-side versioning. While already achievable by overriding the module install destination, getting the concept of a version into PsGet is important, and will improve NuGet integration too.
    2. There is still a lot of work to be done to enable authors to more easily publish their modules. I've found publishing my own modules on GitHub and letting PsGet use the zipballs works well but it doesn't handle modules than require compilation (eg implemented with C#).
    3. I still want to add opt-in support for marking downloaded modules as originating from the Internet Zone for those who want to use the features of Execution Policy. I believe the intent of Execution Policy is misunderstood by many and it serves a useful purpose. I might blog about this one day. This is quite a low priority feature though, especially as I recently discovered PSv2 ignores the zone on binary files.
I hope you'll take a look at PsGet for managing installation of your PowerShell modules and provide your feedback to Mike, I and other contributors via the [PsGet Issues list on GitHub](https://github.com/psget/psget/issues).
