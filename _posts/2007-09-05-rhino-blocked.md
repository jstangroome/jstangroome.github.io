---
layout: post
permalink: http://blog.stangroome.com/?p=59
title: Rhino.Blocked
description: None
date: 2007-09-05 13:19:21 -0000
last_modified_at: 2007-09-05 13:19:21 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I tried using [Rhino.Mocks](http://ayende.com/projects/rhino-mocks.aspx) in production test code for the first time the other day. I downloaded the latest version from Oren Eini's website, unzipped it into source control, and added a reference to my project.

I wrote a quick test utilising the interface mocking features and hit Ctrl+Alt+X to run the test. SecurityException! The message, which I've since forgotten, was complaining that the referenced assembly (ie Rhino.Mocks.dll) could not be loaded.

A quick Google provided the solution, you need to ensure you've unblocked any files you have downloaded from the Internet or a network machine. Now I'm happily mocking away.
