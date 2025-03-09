---
layout: post
permalink: http://blog.stangroome.com/?p=62
title: Debug.Assert and Team Build
description: None
date: 2007-09-03 22:04:09 -0000
last_modified_at: 2007-09-03 22:04:09 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Today I was setting up a nightly build for a new project. After creating the Team Build Type and writing the script to trigger the build on the build machine, I ran the script to ensure it would work. As usual Team Build retrieved the sources, compiled the projects and began running the tests. Half an hour passed and the tests were still running.

I knew the tests couldn't take that long so I stopped the build and opened the solution in Visual Studio to run the tests locally. One of the tests managed to create just the right combination of values in a project class to violate what should otherwise be a class invariant. This particular invariant was verified by a call to Debug.Assert inside the production code.

It has been a very long time since I have written a Debug.Assert statement with an expression that failed the assertion. Today I was reminded that it does not simply throw some kind of AssertException when the expression evaluates to False. Instead a rather hefty dialog box is presented to the user with a full stack trace of the code that failed and a choice of options to proceed.

The problem is that tests running under the service of the build server don't have a visible window station to display the dialog box and there is nothing to automatically click the buttons so the build just hangs indefinitely.

For now I'm just replacing any Debug.Assert statements I find with exception throwing or additional unit tests as appropriate for each case. It may be possible to modify the app.config file to use a different listener to redirect the Debug.Assert output to something non-blocking but that is going to require some investigation.
