---
layout: post
permalink: http://blog.stangroome.com/?p=142
title: Loose Thread
description: None
date: 2005-12-08 00:43:34 -0000
last_modified_at: 2005-12-08 00:43:34 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
It has been a while since my last post because I have been busy. However, I thought I'd better write something about my recent experiences with Visual Studio 2005 and version 2.0 of the .NET Framework. Before I tell you about my first struggle with the new development system, I must say that everything else has been a joy to work with.  
  
I was porting a decent-sized VB.NET 2003 Windows Forms application to .NET 2.0 and updating all the deprecated classes and functions to the new preferred alternatives. There weren't too many problems and I finally cleared all the build errors and warnings. Sadly, not everything was perfect.  
  
Back in VS2003, I had put some effort into making most of the SimpleMAPI functions accessible to my .NET application, mainly for the purpose of opening a new message window with the recipient and subject pre-filled and an attachment ready to go. Not too hard once you know how each Win32 API parameter maps to each .NET type. One annoyance with this SimpleMAPI feature was that showing a new message window was a blocking call that didn't return until the user sent the message or closed the window. I threw together some quick code to spawn a new background thread to make the call and everything was perfect. Until VS2005...  
  
Suddenly, my application, recompiled for .NET 2.0, was complaining about a MAPI logon error. I knew VS2005 had introduced mechanisms to prevent Forms being accessed on other threads so I ruled that out. I also tried different Outlook and Outlook Express configurations. Finally I tried making the call on the main thread and it worked again, but why? I scoured Google Groups for a while and picked up on some obscure references to COM thread apartments.  
  
According to the new documentation, VS2005, by default, creates all new threads with a multithreaded apartment state (MTA). SimpleMAPI (and other COMish APIs) don't like this. Inserting a single line to change to a single-threaded apartment (STA) before starting the new thread solved the logon error problem. I haven't found any official documentation yet but my conclusions would suggest that previous versions of .NET either defaulted to an STA or it was dependant on context.  
  
Hopefully this post will save someone else the same headache I endured.  
