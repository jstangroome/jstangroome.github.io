---
layout: post
permalink: http://blog.stangroome.com/?p=37
title: Why Jon Galloway Is Horribly Wrong About Non-Admin
description: None
date: 2007-12-16 07:48:30 -0000
last_modified_at: 2007-12-16 07:48:30 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
[Jon Galloway](http://weblogs.asp.net/jgalloway/default.aspx) and [Jeff Atwood](http://www.codinghorror.com/blog/) have been having a debate. Mostly the debate has been about anti-virus software, which is a topic that I have no strong feelings on one way or the other.

However, talking about anti-virus involves talking about security and risks and inevitably the topic of running as an administrator versus a limited user account gets mentioned.

Jon, though, more than just mentions running as a non-administrative user, he labels it as "[just about impossible](http://weblogs.asp.net/jgalloway/archive/2007/12/13/why-codinghorror-is-horribly-wrong-about-blacklists-and-virus-scanners.aspx)", and the benefits of living in a non-administrator environment is something I do have a strong opinion about.

I am a .NET developer. I develop console applications, windows services, websites, and smart clients. I do it all, not as an Administrator and not as a Power User, but I do it all as a standard user. I've been doing it this way since [March 2006](http://www.codeassassin.com/blog/PermaLink,guid,dcef5004-ecb0-4e2a-ad64-abf9a3adb604.aspx) and have never gone back.

I don't cheat either. Microsoft suggests running Visual Studio 2005 with increased privileges to avoid various issues. Bah! It works fine as non-admin. The only programs I run with increased privileges are installation programs... and they don't come along very often.

I'm not just using Visual Studio either. I sync my iPod with iTunes and keep my GPS up to date with TomTom Home. I use all the Office apps and I mount CD and DVD images with Daemon Tools. I write scripts in PowerShell and I test beta software in Virtual PC. I have a local install of SQL Server too.

It isn't just me either. Within six months, the other two developers I work with had joined my non-admin club after I had demonstrated not only how painless it can be but how it helps to ensure the software we develop will work when deployed to our clients' restrictive domain environments. Most of all I demonstrated how to be part of the solution to break the cycle of software that requires administrator privileges.

Ok, so far all these people I've mentioned who are running as non-admin could be considered very capable computer users and aren't the same as most people. Fair enough but how about the other less computer-savvy people I know who have been educated on how to work as a non-admin. People with jobs like call centre operator, machinist, automotive-electrician, and cook.

These are people running Windows XP to send faxes, play games, and surf the Internet, and most of them still type with one finger. Switching to an administrator account for occasional tasks is just a Fast User Switch away or, on domain-connected PCs, using the simple [MakeMeAdmin](http://nonadmin.editme.com/MakeMeAdmin) tool.

Vista users get it even easier when an elevation prompt appears asking for an admin password when they need to install software or change a system control panel setting. I just don't understand why people think it is so difficult. And to be clear, there is no hidden magic here, I'm not touching the default ACLs on my file system or registry either.

To come back to where the debate began, my point is that running as a non-admin is not difficult and does not restrict productivity, but it doesn't necessarily help the anti-virus or malware situation either. Some [basic tests](http://www.codinghorror.com/blog/archives/000891.html) do show that it helps in some common infection situations, but I'm confident that I would be able to easily write non-admin compatible malware if needed.

So, Jon, feel free to argue about the effectiveness of anti-virus products or how much being a non-admin protects from malware, but don't tell me non-admin is as usable as unplugging your computer. And Jeff, if you're going to suggest non-admin as a good option, at least lead by example.

By the way, I run 64-bit Windows at work and home too, just to reduce the list of compatible software even further.
