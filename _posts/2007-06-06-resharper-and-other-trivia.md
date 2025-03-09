---
layout: post
permalink: http://blog.stangroome.com/?p=107
title: ReSharper And Other Trivia
description: None
date: 2007-06-05 21:24:12 -0000
last_modified_at: 2007-06-05 21:24:12 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
As Jim has recently explained, [ReSharper 3.0 is now available in beta](http://www.nervoustych.com/blog/PermaLink,guid,fd89afbd-1d6e-4e19-bf18-8ca35dc16327.aspx) and for the first time supports VB.NET. As the majority of the code I work with each day is VB, I have, also for the first time, installed ReSharper. I am enjoying the experience so far and it works well across both VB and C# projects in the same solution.

However, as [I've mentioned before](http://www.codeassassin.com/blog/PermaLink,guid,9028b79a-a594-48a4-9b70-1b25977f9b6a.aspx), I use C# projects to test VB production code and ReSharper was failing to consider InternalsVisibleTo attributes and incorrectly marking code in my unit tests as erroneous. Thankfully Visual Studio still built and executed the code fine. Also very impressive was how quickly the guys at Jetbrains had solved the bug once I had emailed them. Try the [nightly builds here](http://www.jetbrains.net/confluence/display/ReSharper/Nightly+Builds) for the latest version.

As a side note, if you're working with the InternalsVisibleTo attribute, you may find fellow Australian developer [David Kean](http://davidkean.net/)'s [generator](http://davidkean.net/archive/2005/10/06/1183.aspx) very useful for extracting the rather long assembly PublicKey.

Now, if I can just convince ReSharper to give back my [F12](http://blogs.msdn.com/lukeh/archive/2007/06/04/f12-go-to-definition.aspx) key.
