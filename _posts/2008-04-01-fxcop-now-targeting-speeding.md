---
layout: post
permalink: http://blog.stangroome.com/?p=31
title: FxCop Now Targeting Speeding
description: None
date: 2008-03-31 23:39:34 -0000
last_modified_at: 2008-03-31 23:39:34 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Almost two months ago, at the [February Adelaide Geek Dinner](http://www.codeassassin.com/blog/PermaLink,guid,3d03ace8-1a30-4437-9152-275d08d7e7d3.aspx), I was expressing my frustration at one of my Visual Studio solutions taking too long to build and how I would like Visual Studio to build using multiple processors just like the new [MSBuild /m parameter](http://msdn2.microsoft.com/en-us/library/bb651793.aspx).

[Paul Stovell](http://www.paulstovell.com/blog/) made the comment that even with the improvement that multi-core builds was giving me, my solution really shouldn't be taking that long to build. Given that Paul wasn't familiar with my particular project layout and I naturally didn't have a copy with me, the conversation quickly went onto other topics.

However, Paul's comment stayed with me for days after, bugging me every time I waited for the latest build to complete. Then, while staring at the VS Output window during a build, I noticed that most of the time seemed to be spent running FxCop on each project.

I decided to rebuild the solution but this time disabling code analysis via the appropriate build switch. I watched the build time drop from 40 seconds to just 10 seconds by skipping the FxCop process.

Excellent! But given that our entire team runs with [Option Strict On](http://msdn2.microsoft.com/en-us/library/zcd4xwzs.aspx), [Treat Warnings As Errors](http://msdn2.microsoft.com/en-us/library/406xhdz3.aspx), and the [Code Analysis Check-in Policy](http://msdn2.microsoft.com/en-us/library/ms182075.aspx), how could I possibly revert to such a lax build process for the sake of decreased build time?

The answer is to disable Code Analysis in each project's settings (and unfortunately the check-in policy too) but leave it enabled in the Team Build script so it runs and gets reported via the continuous integration build that runs after each check-in. Luckily we've also been running with FxCop for so long that we tend to avoid writing code that would cause violations in the first place.

I met with Paul over the weekend and mentioned my success with better build times. When I told him I had been running FxCop with every build, he just laughed, amazed.
