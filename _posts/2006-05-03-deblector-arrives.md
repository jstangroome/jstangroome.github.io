---
layout: post
permalink: http://blog.stangroome.com/2006/05/03/deblector-arrives/
title: Deblector arrives
description: None
date: 2006-05-02 14:55:35 -0000
last_modified_at: 2010-07-20 00:05:27 -0000
publish: true
pin: false
categories:
- Dot Net
tags: []
---
[Felice Pollano](http://www.felicepollano.com/) has just released the [first version](http://www.felicepollano.com/PermaLink,guid,1c863e69-1b56-4cfa-a60b-203e1127b8bd.aspx) of the new Deblector addin for debugging within [Reflector](http://www.aisto.com/Roeder/DotNet/). It's only alpha at the moment and has some obvious bugs but it is very promising. In the past I have looked at having a Virtual PC with a hacked rebuild of the core framework with debugging enabled. Now I can step into framework code just as easily within the familar Reflector interface. I don't have much experience with IL so I was a little confused at first when multiple lines of IL seemed to execute at once but I quickly realised they all applied to method call setup. I suspect, if possible, we may even be able step through the framework viewed in our chosen language. Keep an eye on this one.
