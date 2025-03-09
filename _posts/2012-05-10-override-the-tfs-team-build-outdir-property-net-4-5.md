---
permalink: http://blog.stangroome.com/2012/05/10/override-the-tfs-team-build-outdir-property-net-4-5/
title: Override the TFS Team Build OutDir property in .NET 4.5
description: None
date: 2012-05-10 05:35:14 -0000
last_modified_at: 2014-02-10 09:02:19 -0000
publish: true
pin: false
categories:
- MSBuild
- TFS
tags:
- OutDir
---
**Update:[with Team Build 2013 it is easier still](http://blog.stangroome.com/2014/02/10/override-the-tfs-team-build-outdir-property-in-tfs-2013/ "Override the TFS Team Build OutDir property in TFS 2013").** I've [blogged before about the challenge of overriding the OutDir MSBuild property](http://blog.codeassassin.com/2012/02/03/override-the-tfs-team-build-outdir-property/) set by Team Build but this hassle is gone in version 4.5 of the .NET Framework. I stumbled upon a change to the core Microsoft.Common.targets file while trying to understand some build issues with a work project and discovered new logic to modify the OutDir property depending on a variety of conditions. I went searching through the rest of the file for other references to OutDir and also discovered at the top of the file, a new attribute on the Project element. This new attribute's name is "[TreatAsLocalProperty](http://msdn.microsoft.com/en-us/library/bcxfsh87\(v=vs.110\).aspx)" and it's value is simply "OutDir". As at the time of posting this blog entry I could not find any documentation of this new functionality but based on my own testing I found that .NET Framework 4.5 now supports:
    * Overriding the value of an MSBuild property that was specified at the MSBuild command-line by naming that property in the TreatAsLocalProperty attribute at the top of the build project.
    * OutDir can now be specified at the command-line without a trailing slash and it will be corrected for you instead of failing the build.
    * Projects can automatically build to subfolders of the Team Build drop location by setting a new MSBuild property named "GenerateProjectSpecificOutputFolder" to "true".
    * The project-subfolder will be named the same as the project file but can be overridden by specifying an alternate value for the "ProjectName" MSBuild property.
    * The OutDir property can now be overridden in whatever custom way you like without modification of the Team Build workflow xaml or using a before-solution targets file.
And because .NET 4.5 is an addition to .NET 4 in the same way .NET 3.5 and .NET 3.0 were to .NET 2.0, your existing .NET 4/VS2010 projects can benefit from this new build-time functionality without taking on new run-time dependencies ([with some exceptions](http://msdn.microsoft.com/en-us/library/hh367887\(v=vs.110\).aspx)). Here is a screenshot for how to configure Team Build 11 or a Team Build 2010 server with .NET 4.5 installed to create per-project folders in the build drop: [![](http://blog.stangroome.com/wp-content/uploads/2012/05/generateprojectspecificoutputfolder.png)](http://blog.stangroome.com/wp-content/uploads/2012/05/generateprojectspecificoutputfolder.png)
