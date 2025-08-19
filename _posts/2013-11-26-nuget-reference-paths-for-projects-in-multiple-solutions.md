---
permalink: /2013/11/26/nuget-reference-paths-for-projects-in-multiple-solutions/
title: NuGet Reference Paths for Projects in Multiple Solutions
description: None
date: 2013-11-26 09:11:13 -0000
last_modified_at: 2013-11-26 09:11:13 -0000
publish: true
pin: false
categories:
- Nuget
tags:
- NuGet
---
This year I have been working with a code base that exhibits Visual Studio projects with three characteristics:
    1. The project references a NuGet package.
    2. The project is included in more than one Visual Studio solution.
    3. The solution files are located in different folders.
I'm not sure how common this scenario is. A few different threads on the NuGet CodePlex site suggests at least some other people are wrestling with it. Personally I endeavour to structure a code base to avoid sharing projects between solutions but for old, high-coupled code this can be difficult to achieve. The problem with this scenario is with the relative paths used to resolve the assemblies within the referenced NuGet package when building the project in clean or constrained working folders - such as on a build agent or when someone first clones a repository. When a NuGet package is installed or updated in a project, the path to the package assemblies are specified relative to the /packages/ folder of the currently open solution. However, when another solution including the same project is built, the assembly won't be resolved either because the first solution's /packages/ folder is not present in the workspace and the NuGet Package Restore workflow has put the assemblies in the second solution's /packages/ folder. The existing attempts to solve this issue, and the same way I approached the problem originally, tend to be focused on writing reference paths relative to an MSBuild property like $(SolutionDir) or $(PackageDir) which then allows the path to be resolved correctly at build time. If I understand correctly, this approach has been rejected from becoming part of the official NuGet application because it doesn't handle the scenario where a project is being built directly, not being built as part of a solution - something I also avoid generally. Last week I had an idea to tackle the problem dynamically at build time instead of when the reference path is written to the project file. My solution is to introduce (yet another) NuGet package to the affected projects as a development-only dependency. I call this the NugetReferenceHintPathRewrite package. This package adds an MSBuild targets file to the project which executes just before the standard ResolveAssemblyReferences MSBuild target. When it executes it looks for references that specify a /packages/ folder as part of their path and then replaces the part of the path up to and including the /packages/ folder with the a new path to the currently building solution's packages folder. This rewrite is done to the MSBuild Items in-process and does not modify the project file on disk. The main benefit of this dynamic build-time approach is that I don't have to worry about new packages being installed or packages being updated (ie re-installed) and the paths in the project file being set to the "wrong" path because someone else forgot to fix it before committing. You can find the [NuGetReferenceHintPathRewrite package on NuGet.org](https://www.nuget.org/packages/NuGetReferenceHintPathRewrite).
