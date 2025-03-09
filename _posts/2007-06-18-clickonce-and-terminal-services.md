---
layout: post
permalink: http://blog.stangroome.com/2007/06/18/clickonce-and-terminal-services/
title: ClickOnce and Terminal Services
description: None
date: 2007-06-18 10:40:42 -0000
last_modified_at: 2010-07-20 00:05:14 -0000
publish: true
pin: false
categories:
- Dot Net
tags: []
---
Under just the right conditions that I have been lucky enough to meet, .NET applications deployed by ClickOnce will not start under a Terminal Services session. At the heart of the problem is this error message: Shortcut activation from http/https or UNC sources is not allowed. I could not find anything in Google, on the MSDN forums or in the Microsoft Support Knowledge Base. With some trial and error and a little reflection I've tracked it down to this very special combination:

* ClickOnce offline install mode is enabled
* and the application is started via the Start Menu shortcut
* and the user is logged into a Terminal Services session
* and Terminal Services is redirecting the Start Menu to a UNC.

Changing any one of these conditions is enough to avoid the problem and would probably explain why it is documented anywhere. The error message is triggered by code in the System.Deployment assembly and it it cannot be overridden by a system setting either. Redirecting the Terminal Services Start Menu to a UNC is also a common practice when running multiple terminal servers, or a shared Start Menu for all users (as was my case). I will be recommending users simply start the apps via the same ClickOnce deployment URL that was used for the initial install and try to disable TS folder redirection where possible. It seems ClickOnce uses the shortcut location later in the bootstrap process and the problem exists in Orcas too so I don't think Microsoft will be fixing this one.
