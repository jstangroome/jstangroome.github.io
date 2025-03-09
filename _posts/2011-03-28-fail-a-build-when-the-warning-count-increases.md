---
layout: post
permalink: http://blog.stangroome.com/2011/03/28/fail-a-build-when-the-warning-count-increases/
title: Fail a build when the warning count increases
description: None
date: 2011-03-27 22:28:44 -0000
last_modified_at: 2011-03-27 22:28:44 -0000
publish: true
pin: false
categories:
- TFS
tags: []
---
I like to set the "Treat warnings as errors" option in all my Visual Studio projects to "All" to ensure that the code stays as clean and maintainable as possible and issues that may not be noticed until runtime are instead discovered at compile time. However, on existing projects with a large number of compiler warnings already present in the code base, turning this option on will immediately fail all the CI builds and either create a lot of work for the team before the builds are passing again or the team will start ignoring the failed builds. Neither is a desirable situation. On the other hand, unless someone is actively monitoring the warnings and notifying the team when new compile warnings are introduced, the technical debt is just going to increase. To address this I thought it would be useful to extend the default build process template in TFS 2010 to compare the current build's warning count with the warning count from the previous build and fail the build if the number of warnings has increased. The Xaml required for this can be[found here](https://gist.github.com/889708). Hopefully this strategy will result in the team slowly decreasing the warning count down to zero and then the "Treat warnings as errors" option can be enabled to prevent new compiler warnings being introduced to the code base. At the moment this is a very naive implementation - if an increase in warnings fails one build, subsequent builds will pass unless the warning count increases again. I have two ideas for improving this:

  1. Compare the current warning count against the minimum warning count of all previous builds.
  2. Fail if the minimum warning count has not decreased within a specified time period (eg two weeks).

If I implement either of these two ideas, I'll update this post but they should both be quite easy to do-it-yourself.
