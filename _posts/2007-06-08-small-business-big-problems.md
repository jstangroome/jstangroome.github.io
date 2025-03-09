---
layout: post
permalink: http://blog.stangroome.com/?p=104
title: Small Business, Big Problems
description: None
date: 2007-06-08 07:10:17 -0000
last_modified_at: 2007-06-08 07:10:17 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I've had the pleasure of chasing down some complications on a recent Small Business Server 2003 R1 deployment for a client. As is our preferred style for building new machines, we installed Windows then we ran Microsoft Update over and over, rebooting in between as necessary until every available critical and optional update had been installed.

We then proceeded to deploy our software's prerequisites to the server followed by the software itself, and lastly configure the server ready for deployment to the client's network. This last part was interesting... the SBS licensing GUI would crash when we attempted to start it. Some extensive Googling found this [KB article](http://support.microsoft.com/kb/897342 "Known issues that may occur if you install Windows Server 2003 SP1 on Windows Small Business Server 2003") suggesting to disable DEP for the offending program. Problem solved.

A little later the program for creating Active Directory Users was crashing exactly the same way. I couldn't find any KB articles for this program but I tried disabling DEP for the entire system and rebooted. Problem solved again. However, I wasn't happy that turning off DEP was right.

I spent an evening wandering through forums and blogs about SBS and eventually concluded that SBS SP1 was supposed to solve the problems. Of course, we had installed all the Microsoft Updates so that should already be fixed right? Sadly, no.

It turns out that because Service Pack 1 for SBS2003 apparently changes too much it doesn't get offered via Windows Update, you have to find it, download it, and install it manually. To make things worse, SP1 for SBS is supposed to be installed before SP2 for Windows Server 2003 but Microsoft Update had already installed that.

Several hours later we had uninstalled Windows Server SP2, installed the multi-file SBS SP1 update, reinstall Windows Server SP2, broken the .NET 2.0 framework, repaired that, broken SQL 2005 Reporting Services then reinstalled that, and finally reapplied SQL 2005 SP2.

Next time I think we'll have to remember to install SBS 2003 R2.
