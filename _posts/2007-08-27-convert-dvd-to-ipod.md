---
layout: post
permalink: http://blog.stangroome.com/?p=63
title: Convert DVD to iPod
description: None
date: 2007-08-27 13:00:56 -0000
last_modified_at: 2007-08-27 13:00:56 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I have just spend two weeks on holiday and before I left I wanted to transfer some of my DVDs to my iPod Video to keep my fiancee and I entertained whilst traveling to our destination. Unfortunately I didn't really think about this soon enough and didn't have time to get it organised. Now that I'm back home though I've been able to look into it further.

The biggest problem I found with converting DVDs to an iPod suitable format is that the movie studios don't seem to want to allow it. This results in either a complicated list of steps involving multiple tricky applications or purchasing an all-on-one solution from a questionable overseas company. Luckily though, Hanselman recently updated his [Tools List for Windows](http://www.hanselman.com/tools), which included a link to [Videora Converters](http://www.videora.com/en-us/Converter/), a suite of video transcoding programs available for free. If Hanselman recommends it and I don't have to provide payment details, it's worth a try.

I downloaded their iPod converter and started following their slightly outdated [guide for converting DVDs](http://www.videora.com/en-us/Converter/guides.html#1000). I grabbed my Futurama box set and put the first disc in. The first stage involves using the getting-harder-to-find DVD Decrypter to rip and decrypt the appropriate video and audio stream without all the extra DVD menu junk. Thankfully I'm familiar with DVD Decrypter and even more so that it works fine as non-admin in Vista 64-bit.

The second stage involves installing and running the Videora software, selecting the file that DVD Decrypter produced and waiting for it to convert the MPEG-2 data to Apple's particular MPEG-4 format. This took less than 10 minutes on my Core 2 Duo to convert a 22-minute animated episode into a 137MB MP4 file. When complete, the video played well in the PC QuickTime player and after using iTunes to transfer it to my iPod it played great there too.

Sadly, while the Videora software is free and written in .NET too, it suffers from two major problems. Firstly, it is so filled with advertising in the program itself that it's hard to see where to start and I began to wonder if I'd just voluntarily installed malware. Secondly, the software is designed to run as an Administrator, trying to write configuration data to Program Files, and under Vista, even as an admin, it crashes hard on exit.

Ultimately, if you want some favourite DVD movies or episodes on your iPod, this is probably the easiest way.
