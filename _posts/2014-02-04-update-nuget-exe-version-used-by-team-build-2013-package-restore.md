---
layout: post
permalink: http://blog.stangroome.com/2014/02/04/update-nuget-exe-version-used-by-team-build-2013-package-restore/
title: Update NuGet.exe version used by Team Build 2013 Package Restore
description: None
date: 2014-02-04 05:04:17 -0000
last_modified_at: 2014-02-04 20:47:19 -0000
publish: true
pin: false
categories:
- Nuget
- TFS
tags:
- NuGet
---
Since NuGet 2.7, there is a [new approach to Package Restore](http://docs.nuget.org/docs/release-notes/nuget-2.7#The_New_Package_Restore_Workflow). In short, it involves executing "nuget.exe restore" before building the solution or project, instead of having each project import the "nuget.targets" file. This new restore workflow solves a number of issues, especially with packages containing MSBuild customizations, but also with parallel builds conflicting when performing the restore in parallel. Additionally, Team Foundation Server 2013's Team Build implements this new Package Restore workflow in its default build process templates for both TFVC and Git repositories without any effort. This functionality is implemented care of the new RunMSBuild workflow activity (not to be confused with the [original MSBuild workflow activity](http://msdn.microsoft.com/en-us/library/gg265783.aspx#Activity_MSBuild)). The RunMSBuild activity internally uses another new activity named "NuGetRestore", which is also conveniently a public type you can use directly in customized build process templates. The NuGetRestore activity simply runs "nuget.exe" via the [InvokeProcess activity](http://msdn.microsoft.com/en-us/library/gg265783.aspx#Activity_InvokeProcess) to perform the real work, so there is no special TFS-only behaviour. However, by default, the copy of "nuget.exe" that is used for the restore is located in the same folder as the assembly declaring the NuGetRestore activity (Microsoft.TeamFoundation.Build.Activities.dll) typically located in "C:\Program Files\Microsoft Team Foundation Server 12.0\Tools". The version of this "nuget.exe" that ships with TFS 2013 RTM is version 2.7 but there is a good chance there will regularly be a newer NuGet available than the version shipped with Team Build, and with features you need or want. For example, version 2.8 was recently released and the [new Fallback to Local Cache feature](http://docs.nuget.org/docs/release-notes/nuget-2.8#Fallback_to_Local_Cache) would be one handy way to improve build resiliency when the build agent can't always connect to a NuGet repository. I've done some research and I have found there are basically two options available for using a newer version of NuGet in your Team Builds now:
    1. Remote to each Team Build Agent with local Administrator privileges, and execute "nuget.exe update -self" on the file located in the TFS Tools folder mentioned above, or ...
    2. Customize your build process XAML file in two places:
      1. Set the "RestoreNuGetPackages" argument to "false" on the RunMSBuild activity to avoid using the default "nuget.exe".
      2. Insert the NuGetRestore activity immediately before RunMSBuild set the "ToolPath" argument to the location of the desired version of "nuget.exe" to use.
With any luck, each future TFS update will ship with the most recent version of NuGet for those builds that can wait.
