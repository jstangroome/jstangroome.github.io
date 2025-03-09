---
layout: post
permalink: http://blog.stangroome.com/?p=148
title: Anyone heard of design guidelines?
description: None
date: 2005-05-04 16:01:39 -0000
last_modified_at: 2005-05-04 16:01:39 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

I often get the feeling that I am the only person who has setup Windows 2K/XP computers with limited-access users. Does everyone out there really configure every user as an Administrator? The majority of application software and games available today suggest this is true.

Microsoft published a document entitled "Designed for Windows XP Application Specification" about three years ago. I can only assume that nobody has read it or any of the other related publications or API documentation. On page 37 of this document is Section 3.0, "Data and Settings Management". Here it quite clearly explains where applications need to save user related data and how. The days of Windows 3.1 are over. We have a Program Files folder with security permissions denying write access to non-Administrators. This has been the situation since Windows 2000 (maybe Windows NT 4.0 also?). Yet too many programs attempt to store user configuration information in the same folder as the executable itself.

On my home PC, I have a user account for myself and a user account for my girlfriend, Libby. Libby's account is a limited user account and this breaks a lot of software. She can't use WinAmp without modified permissions on the application's install folder. Also, we each end up inheriting the configuration changes made by the other. Many games that Libby likes to play (including the new and fantastic Yourself Fitness!) have problems too. These games try to store game progress data in the game's install folder. As a result Libby has to start from the beginning everytime unless I change the default permissions.

As the administrator it is necessary for me to install any software that Libby requests. If I am happy it isn't something harmful that she has downloaded I go ahead. Unfortunately it seems that a lot of software won't install to multiple users either. It is not just that the setup program simply didn't put the shortcuts in the All Users start menu. If I create the appropriate shortcuts, the software still won't run under a non-privileged account.

I guess the problem is a combination of ignorance, failure to test restricted user scenarios, and simply considering the problem insignificant. WinAmp has been around for a long time. Version 5 was released not too long ago. However, discussions regarding multi-user support in WinAmp on the WinAmp developer forums have been dismissed as not important.

Everywhere we look today, the focus is on security but if even the most secure and stable software won't work for users other than administrators, what is the point?

]]>
