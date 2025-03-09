---
layout: post
permalink: http://blog.stangroome.com/?p=144
title: Truly Resourceful
description: None
date: 2005-06-15 14:28:50 -0000
last_modified_at: 2005-06-15 14:28:50 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

While Googling for something quite different, I stumbled across [an article](http://www.codeproject.com/cs/miscctrl/MessageBoxIndirectCS.asp) on [The Code Project](http://www.codeproject.com) by Scott McMaster. The article covers almost everything you would want to know about using the Win32 MessageBoxIndirect API from .NET. Specifically, he discusses the problems with using custom icons considering .NET resources and Win32 resources are completely different. One of Scott's solutions is to create a small unmanaged Visual C++ project purely to host the icons as Win32 resources.

However, Scott's MessageBoxIndirect wrapper exposes most of the gory details of the API and expects the caller to pass in a handle to the C++ resource DLL as an IntPtr. I decided to tidy this process for the wrapper I built and added an IDisposable implementation to my class. This class accepts the path to the DLL as a String and the resource ID as an Int32 and handles the calling of the LoadLibraryEx and FreeLibrary APIs appropriately.

]]>
