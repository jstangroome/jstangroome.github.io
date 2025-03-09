---
layout: post
permalink: http://blog.stangroome.com/?p=89
title: Crashing Visual Studio Unit Testing
description: None
date: 2007-06-23 10:53:30 -0000
last_modified_at: 2007-06-23 10:53:30 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Yesterday I managed to crash vstesthost.exe, the program that runs Visual Studio Unit Tests when you press Ctrl+Shift+X. A few tests in my suite would run and pass, then BAM, fatal error. I ran the tests again, this time in debug mode and the tests broke into the debugger with a StackOverflowException, due to a poorly implemented interface in a mock object.

According to some searching, some exceptions including [StackOverflowException](http://msdn2.microsoft.com/en-us/library/system.stackoverflowexception.aspx), just cannot be caught in .NET 2.0 and I'm not sure it would be smart to try. Unfortunately when it happens in testing, the whole suite fails. In my case, I was able to pave over my mistake in the code and move on with my life.

The vstesthost crashing problem also happens when an exception is through on a secondary thread though. Testing asynchronous UI or background processing code could also kill the process when any exceptions occur, and these might be expected exceptions for a particular tests.

I haven't needed to use it yet, but there is a workaround to avoid the whole test suite falling over. It involves editing the vstesthost.exe.config file to enable the [legacy unhandled exception policy](http://msdn2.microsoft.com/en-us/library/ms228965.aspx) as used by the earlier CLR versions. You will need to be careful to synchronise your threads though to ensure any exceptions are assigned to the correct test in the results.
