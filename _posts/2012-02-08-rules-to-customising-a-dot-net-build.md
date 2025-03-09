---
layout: post
permalink: http://blog.stangroome.com/2012/02/08/rules-to-customising-a-dot-net-build/
title: Rules to Customising a .NET Build
description: None
date: 2012-02-08 10:26:56 -0000
last_modified_at: 2014-02-06 04:50:35 -0000
publish: true
pin: false
categories:
- MSBuild
- TFS
- Visual Studio
tags: []
---
It doesn't take long before any reasonable software project requires a build process that does more than the IDE's default Build command. When developing software based on the .NET platform there are several different ways to extend the build process beyond the standard Visual Studio compilation steps. Here is a list for choosing the best place to insert custom build steps into the process, with the simplest first and the least desirable last:
    1. [Project pre- and post-build events](http://msdn.microsoft.com/en-us/library/ke5z92ks.aspx): There is a GUI, you have access to a handful of project variables, can perform the same tasks as a Windows Batch script, and still works with Visual Studio's F5 build and run experience. Unfortunately only a failure of the last command in a batch will fail the build and the pre-build event happens before project references are resolved so avoid using it to copy dependencies.
    2. [MSBuild target overrides within the project files](http://msdn.microsoft.com/en-us/library/ms366724.aspx) (eg BeforeBuild, AfterBuild): You get the full power of MSBuild, and still works with the F5 build and run experience but it’s a little concealed from those who don’t know to look there.
    3. [MSBuild imports](http://msdn.microsoft.com/en-us/library/92x05xfs.aspx) within the project files: As above but more maintainable and reusable. That extra MSBuild file in the solution might grab some more attention than an inline target override. If the customization is generic enough, using it can become simpler by publishing it as a Nuget package. Using MSBuild 4's before- and after- solution targets files fit here also.
    4. A parent MSBuild file to build the solution and perform additional tasks: Still works locally in the development environment but no longer plays nicely with F5. Someone is going to have to open a command prompt.
    5. Customising the CI server's build pipeline. For TFS this means Team Build Process Template customization: Using the Workflow Designer destroys version history of the template file so prepare to get your hands dirty in XAML and VB, and all building requires a Team Build Agent so the feedback loop for testing customizations just exploded. For TeamCity this means your build customizations are no longer version controlled with the rest of the source, you also take a hit on the feedback loop.
Items 2 through 4 above are all MSBuild and where most build customization should live. Here is a list for choosing the best technique for implementing your build customizations within MSBuild, again in order of preferred first:
    1. [Out of the box MSBuild tasks](http://msdn.microsoft.com/en-us/library/7z253716.aspx): find neat ways to leverage them without taking any new dependencies. If you get to know the MSBuild well, an amazing amount can be achieved.
    2. Use the [Exec MSBuild task](http://msdn.microsoft.com/en-us/library/x8zx72cd.aspx) to call out to existing processes: the OS and .NET SDK is full of handy tools that don’t need to be installed on your build server first. Other tools can be source controlled too.
    3. [MSBuild 4 Inline Tasks](http://msdn.microsoft.com/en-us/library/dd722601.aspx): New to MSBuild v4, custom MSBuild tasks can be defined using C# directly inside the MSBuild project. A lot more power, still no issues deploying build customisations to developers or build servers .
    4. Run PowerShell scripts via the Exec task: Modern OSes have PowerShell installed by default, you have all the power of .NET and more and still don’t have to deploy anything to support your customisations.
    5. Compiled MSBuild Tasks: Too far, go back. Now you need a process to compile your customisations then install or version-control the binaries where other developers and build servers can use them.
If after all these options you do find yourself customising the CI server's build pipeline, the very least desirable situation is then using custom extensions to that pipeline. For TFS, this means custom Workflow Activities, for TeamCity, this means custom plugins.  Make sure all other options are exhausted before it comes to this.
