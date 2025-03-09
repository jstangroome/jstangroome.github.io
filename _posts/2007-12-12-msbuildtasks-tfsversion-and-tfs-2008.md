---
layout: post
permalink: http://blog.stangroome.com/?p=40
title: MSBuildTasks, TfsVersion and TFS 2008
description: None
date: 2007-12-11 21:31:26 -0000
last_modified_at: 2007-12-11 21:31:26 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Jeremy Miller [says it best](http://codebetter.com/blogs/jeremy.miller/archive/2007/12/06/do-you-really-know-where-that-code-has-been.aspx):

_"NEVER build and deploy from a developer environment"_

In his article, Jeremy puts into concrete words all the reasons why I felt deploying from the workstation was a bad idea.

In my current project at work I had an opportunity to setup continuous integration and a good developer workflow from the beginning and I already had building, testing, and deploying automated on the build server. But, in the same article Jeremy shows an example for using source control version labels for .NET assembly version numbers.

Until now we were just using the default compiler generated build and revision numbers, which are days since January 1st 2000 and seconds since midnight respectively if I remember correctly. Unfortunately, while this ensures increasing and unique version numbers, it doesn't help map a deployed assembly back to it's source version.

I decided to find an alternative to Jeremy's CC.NET+NAnt build script to suite our TFS2008+MSBuild environment. I knew replacing the version numbers in the AssemblyInfo.* files would be trivial but I needed to get the TFS changeset from somewhere.

The popular open-source [MSBuild Community Tasks Project](http://msbuildtasks.tigris.org/) already includes a TfsVersion task to grab various TFS property values and make them available to the script. However, I quickly discovered that the TfsVersion task expects the 2005 version of the TFS client libraries.

Although documentation is slim, all the MSBuildTasks source code is available online in a Subversion repository and I was able to download the code and work on porting it to TFS 2008.

Thankfully I didn't have to. All the TFS related code in MSBuildTasks is done via late-bound proxies and there is a TfsLibraryLocation property on the TfsVersion task that can be pointed to the "...\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies\" folder.

I also found Richard Banks' post on [Versioning Builds with TFS and MSBuild](http://richardsbraindump.blogspot.com/2007/07/versioning-builds-with-tfs-and-msbuild.html) to be very helpful for getting the build targets right.
