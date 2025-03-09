---
layout: post
permalink: http://blog.stangroome.com/?p=147
title: Being resourceful
description: None
date: 2005-05-31 01:44:43 -0000
last_modified_at: 2005-05-31 01:44:43 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

Often I feel like I spend too much of my time searching through the framework source and the Win32 API when developing .NET applications. Today was no exception. A colleague suggested that a pair of message boxes displayed the end user in some retail POS software appeared too similar and should have custom icons to help distinguish them.

Windows.Forms.MessageBox provides a wrapper for the Win32 MessageBoxEx function which only supports the four standard message icons. There is however the Win32 MessageBoxIndirect function which is not accessible via the standard .NET framework but does support custom icons. Unfortunately the MessageBoxIndirect function only supports icons embedded as a Win32 resource in the executable and .NET resources are embedded completely differently.

I can add a .rc/.res file to Managed C++ project to be embedded in the executable but the IDE doesn't support .rc/.res files in a C# or VB.NET project (which is the language used by the retail POS software). It is possible to use the command line tools to build the .res file from a .rc file and link it into a C# or VB.NET assembly but this would be awkward to do even with some Visual Studio IDE post-build addin. I could add a Managed C++ project to the solution to provide all the Win32 icon resources but either way this is becoming all too complex for a feature that was destined to be included in a common class library.

Considering .rc/.res files are language independent and are used by the IDE internally to embed the executable file's icon as a Win32 resource this should be possible. I would be very interested if anyone knows how to fudge a VB.NET project to link a .res file into the final output target as part of the standard build process.

On a similar note, I have ordered John Mueller's book ".NET Framework Solutions: In Search of the Lost Win32 API". Hopefully this will be very handy when I tackle the rest of the framework.

]]>
