---
layout: post
permalink: http://blog.stangroome.com/?p=70
title: Visual Studio versus MSBuild
description: None
date: 2007-07-28 01:11:28 -0000
last_modified_at: 2007-07-28 01:11:28 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

[![Hammer](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/VisualStudioversusMSBuild_11304/hammer_1.jpg)](http://www.flickr.com/photos/m2w2/191545978/) I have recently setup a TFS Team Build server for our work projects after realising that the [secondary reference issue](http://sstjean.blogspot.com/2006/11/msbuild-cant-find-secondary-references.html) is not as problematic as first thought. We haven't been using Visual Studio's Remove Unused Reference feature as much as expected and we need to prepare our projects for [Visual Studio 2008](http://msdn2.microsoft.com/en-us/vstudio/aa700831.aspx).

With a build server configured to perform automatic builds every night we are on our way to full continuous integration with builds on check-in. However, there are several other differences between Visual Studio and MSBuild that are creating slight issues for our TFS build agent.

For what I understand of [the situation](http://msdn2.microsoft.com/en-us/library/ms171468\(VS.80\).aspx), Visual Studio utilises an in-process variation of the MSBuild engine to support specific features like background compilation but has failed to maintain full compatibility.

For example, installation of SQL Server 2005 on your development machine will offer four new Business Intelligence project types: Analysis Services, Integration Services, Report Server, and Report Model. None of these project types will build from the command line in MSBuild or therefore Team Build. We have had to move these project into separate solutions that we exclude from the build server's automated processing.

Also, strongly-typed DataSets and other designer-based project items are usually based on a metadata file (like an XSD) and use a custom tool to generate an appropriate VB or C# code file before the project is compiled. The project files contain the information about which generator tool to use to update the code files from the metadata but again MSBuild does not support it. Any changes to the metadata that is checked-in will not be reflected in the Team Build output.

The idea to have an IDE and a build engine based on the same technology and file formats is brilliant. The need to only manage build settings in one place is the biggest reason for not moving to NAnt or another offering where I'd need to maintain both the Visual Studio projects and the third party scripts manually.

Unfortunately I feel that Microsoft made two major mistakes with MSBuild and Visual Studio:

  * APIs were provided to allow Visual Studio to be extended to support new project items and project types but the API doesn't force the extension to be callable by MSBuild.
  * Microsoft then utilised this API to extend Visual Studio for SQL Server projects and others too and also ignored MSBuild compatibility themselves.

]]>
