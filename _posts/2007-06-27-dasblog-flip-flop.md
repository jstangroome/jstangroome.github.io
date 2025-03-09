---
layout: post
permalink: http://blog.stangroome.com/?p=85
title: DasBlog Flip Flop
description: None
date: 2007-06-26 22:25:48 -0000
last_modified_at: 2007-06-26 22:25:48 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

A few months ago I upgraded this web site from the DasBlog 1.8 installed by my ISP to the latest version at the time, 1.9.6. It was remarkably easy to merge my web.config and site.config files and upload all the new binaries.

Yesterday [Scott Hanselman](http://www.hanselman.com/blog/) [announced DasBlog 1.9.7](http://www.hanselman.com/blog/DasBlog197ReleaseFinalASPNET11Version.aspx) had been released. Last night I tried to upgrade again, following a very similar procedure as I did the last time. Being a point release, I expected even less effort and was happy to find that very little merging was required with the config files.

Unfortunately, once I had uploaded the new files to my site I found I could no longer log in. I refreshed the browser several times, edited and uploaded the siteSecurity.config file several times and tried the default file with the admin/admin credentials. Nothing would let me login and all I had was a SecurityFailure message in the event log.

This took an hour or so and I eventually gave up and completely restored the backup I made before attempting the upgrade. Guess I'll have to configure IIS locally and experiment until it works.

On a better note, Scott says this is the final release for ASP.NET 1.1 and another release due very soon will be completely ASP.NET 2.0 and support Medium Trust. I was annoyed last time I downloaded the source and discovered that I would need to install VS2003 to work with it but finally I'll be able to contribute to the project.

]]>
