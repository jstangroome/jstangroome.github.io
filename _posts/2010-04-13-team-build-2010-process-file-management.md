---
layout: post
permalink: http://blog.stangroome.com/2010/04/13/team-build-2010-process-file-management/
title: "Team Build 2010 process file\_management"
description: None
date: 2010-04-12 21:26:51 -0000
last_modified_at: 2010-07-18 08:29:49 -0000
publish: true
pin: false
categories:
- TFS
tags: []
---
When you create a new Team Foundation Server 2010 Team Project from the standard MSF process templates and choose the defaults, you will be given a BuildProcessTemplates folder in your new project’s source control root. In this folder you should discover three Xaml files, corresponding to each of the out-of-the-box build types: default, 2008-compatible build, and Lab build.

If you then create a new Build Definition for your team, the Process page will default to the “Default Template”. If you expand for more details, you’ll see a drop-down of the three choices or a New button. It’s not until you click the New button that it’s obvious you can still put your build files anywhere in source control, just like TFS 2008. Upon creating a new process file you should also discover the drop-down list will include your new file, even if you don’t store it in BuildProcessTemplates folder.

Until your build definitions exceed the limitations of the out-of-the-box process files, it is often fine to use them where they are and just adjust the Parameters property grid. As soon as you want to customise the build process itself, you should move to a more manageable arrangement for these files. My preferred structure is basically to create a new Builds folder under your Trunk folder (or “Main” or whatever you call your branching root) and then branch the base template file (eg DefaultTemplate.xaml) into this new Builds folder with a more descriptive file name (eg MyNightlyBuild.xaml) and customise it there.

Unfortunately, while the Build Definition window has a Copy button to create a new process file to work on in a single step, copying doesn’t maintain a relationship between your new file and the original. This is important because now in 2010, the build files contain the full build process (not just the overrides like TfsBuild.proj files did) and they therefore much more closely resemble the Microsoft.TeamFoundation.Builds.targets file from TFS 2008 and are subject to servicing. By branching, when a service pack introduces new versions of the templates, you will be able to merge the updates through to your customised files. And just like the 2008 targets files, you should avoid reverse-integrating your customisations back to the originals.

In addition to leaving the original files unchanged, there are other reasons why you should put your process files in a Build folder below your trunk:

* When you branch source, you should branch build process too. Keeping them below your branching root makes this automatic.
* If you want to use reference custom build activities, you can no longer just double-click the Xaml file in Source Control to edit it. Instead you need to put your Xaml file in a project that has project references to the activity assemblies. The build folder gives you a perfect place to put the project file and optionally add it to your solution.
* Finally, customised builds will often reference other external files (eg PowerShell scripts) and the build folder is a good place for them too.

If you've found any other good practices with managing TFS 2010 build process files, feel free to leave a comment.
