---
layout: post
permalink: http://blog.stangroome.com/2012/02/03/override-the-tfs-team-build-outdir-property/
title: Override the TFS Team Build OutDir property
description: None
date: 2012-02-03 04:02:37 -0000
last_modified_at: 2014-02-11 20:13:09 -0000
publish: true
pin: false
categories:
- MSBuild
- TFS
tags:
- OutDir
---
**Update:[with .NET 4.5 there is an easier way](http://blog.stangroome.com/2012/05/10/override-the-tfs-team-build-outdir-property-net-4-5/ "Override the TFS Team Build OutDir property in .NET 4.5").** A very common complaint from users of Team Foundation Server's build system is that it changes the folder structure of the project outputs. By default Visual Studio puts all the files in each project's respective /bin/ or /bin/<configuration>/ folder but Team Build just uses a flat folder structure putting all the files in the drop folder root or, again, a /<configuration>/ subfolder in the drop folder, with all project outputs mixed together. Additionally because Team Build achieves this by setting the OutDir property via the MSBuild.exe command-line combined with[MSBuild's property precedence](http://blogs.msdn.com/b/aaronhallberg/archive/2007/07/16/msbuild-property-evaluation.aspx) this value cannot easily be changed from within MSBuild itself and the popular solution is to [edit the Build Process Template *.xaml file to use a different property name](http://blogs.msdn.com/b/jimlamb/archive/2010/04/13/customizableoutdir-in-tfs-2010.aspx). But I prefer not to touch the Workflow unless **absolutely** necessary. Instead, I use both the [Solution Before Target](http://sedodream.com/2010/10/22/MSBuildExtendingTheSolutionBuild.aspx) and the [Inline Task](http://msdn.microsoft.com/en-us/library/dd722601.aspx) features of MSBuild v4 to override the default implementation of the [MSBuild Task](http://msdn.microsoft.com/en-us/library/z7f65y0d.aspx) used to build the individual projects in the solution. In my alternative implementation, I prevent the OutDir property from being passed through and I pass through a property called PreferredOutDir instead which individual projects can use if desired. The first part, substituting the OutDir property for the PreferredOutDir property at the solution level is achieved simply by adding a new file to the directory your solution file resides in. This new file should be named following the pattern "before.<your solution name>.sln.targets", eg for a solution file called "Foo.sln" then new file would be "before.Foo.sln.targets". The contents of this new file should [look like this](https://gist.github.com/1727206#file_before.the_solution.sln.targets). Make sure this new file gets checked-in to source control. The second part, letting each project control its output folder structure, is simply a matter of adding a line to the project's *.csproj or *.vbproj file (depending on the language). Locate the first <PropertyGroup> element inside the project file that doesn't have a Condition attribute specified, and the locate the corresponding </PropertyGroup> closing tag for this element. Immediately above the closing tag add a line something like this:
  
        <OutDir Condition=" '$(PreferredOutDir)' != '' ">$(PreferredOutDir)$(MSBuildProjectName)\</OutDir>

In this example the project will output to the Team Build drop folder under a subfolder named the same as the project file (without the .csproj extension). You might choose a different pattern. Also, Web projects usually create their own output folder under a _PublishedWebSites subfolder of the Team Build drop folder, to maintain this behaviour just set the OutDir property to equal the PreferredOutDir property exactly. You can verify if your changes have worked on your local machine before checking in simply by running MSBuild from the command-line and specifying the OutDir property just like Team Build does, eg:
  
        msbuild Foo.sln /p:OutDir=c:\TestDropFolder\
