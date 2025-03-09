---
layout: post
permalink: http://blog.stangroome.com/?p=99
title: Strength In Numbers
description: None
date: 2007-06-13 22:49:31 -0000
last_modified_at: 2007-06-13 22:49:31 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

Since Visual Studio 2005 has made the whole process much easier, I strong name every assembly I work on. It's one of the first things I do when I add a new project to a solution: Choose a name, set strict compilation warnings, enable Code Analysis, assign a strong name key.

I have used a few open source projects lately and have been disappointed that the binaries haven't always been strong name signed. This means I have to get the source instead, setup a new key and adjust their build script to use it before I can even add it as a reference to my own project.

[![Weight Lifting](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/GiveNamesToTheNameless_8E22/weightlifting_1.jpg)](http://www.flickr.com/photos/giovannijl-s_photohut/361679973/) There are several benefits to using strong names:

  * Optional deployment to the GAC.
  * Satisfy Code Analysis rule [CA2210](http://msdn2.microsoft.com/en-us/library/ms182127\(VS.80\).aspx).
  * Use of the InternalsVisibleTo attribute.
  * Reference from other strong named assemblies.

I don't think I've ever deployed any assemblies to the GAC, so only the last three are relevant for me but I'm sure GAC deployment is important to people writing COM interop code or add-ins for unmanaged applications or it will be important one day when their project gets reused in that context.

The counter-argument is that strong names are supposed to provide security and handing out the private key file with rest of the source would defeat the purpose. However, [Mike Downen](http://blogs.msdn.com/CLRSecurity/), program manager for security on the CLR team, [says this](http://msdn.microsoft.com/msdnmag/issues/06/07/CLRInsideOut/default.aspx#S4):

> _They have no revocation capabilities and they don't expire. Nor do strong names inherently provide any way to securely map a public key to a particular publisher. Thus, strong names should be used very cautiously when making security decisions._

For these reasons I think it is a good idea to strong name all assemblies. Perhaps use a different strong name private key for each program. The owner(s) of an open source project could even retain a private key separate from the one distributed by the open source repository and use it for signing official binary releases.

]]>
