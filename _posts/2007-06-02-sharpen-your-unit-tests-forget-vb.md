---
layout: post
permalink: http://blog.stangroome.com/?p=110
title: Sharpen Your Unit Tests, Forget VB
description: None
date: 2007-06-01 21:24:59 -0000
last_modified_at: 2007-06-01 21:24:59 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

We are entering a new stage of development at work, we've released betas of three modules and are now beginning the initial iterations for two new modules. We are taking this opportunity to push unit testing into our standard development cycle, which is something I've wanted to do for a long time.

With friend and work colleague, [Jim Burger](http://www.nervoustych.com/blog/), leading the charge to unit test and document our common libraries, some "best practices" that suit our project model are starting to reveal themselves.

For each project under test, we create a corresponding test project with the same name suffixed by "Tests". This keeps the projects paired together in the Solution Explorer and creates default namespaces for the projects that are just different enough to avoid clashes.

Within the test project we then create a folder for each class under test and within those folders, we create a unit test class for each function under test. We've found that each function usually requires at least three tests (null guard test, common case, edge case) and giving each function it's own file keeps our tests manageable.

Most importantly, while all our production code is in VB, we write our unit tests in C#. One of the main reasons is that the C# compiler respects the [InternalsVisibleTo](http://msdn2.microsoft.com/en-us/library/system.runtime.compilerservices.internalsvisibletoattribute.aspx) attribute and allows us to test code marked as Friend without resorting to reflection in our tests or putting test-only code in our production projects.

We also get the nice benefit that C# will automatically put classes in namespaces that match the folder structure in the project. This provides a simple mechanism to categorise our tests for running them and viewing the results. C# also tends to produce slightly more succinct code which just feels better as the number of tests grows.

Although both VB and C# have their pros and cons, the majority of the .NET world is C# and so are the job opportunities. As a VB programmer, by using C# for at least some of your work, you make sure you are learning more aspects of .NET and when you get involved in the .NET community you're more likely to appreciate what they're talking about.

* * *

This post brought to you by [Windows Live Writer Beta 2](http://windowslivewriter.spaces.live.com/blog/cns!D85741BB5E0BE8AA!1272.entry).

[![kick it on DotNetKicks.com](http://www.dotnetkicks.com/Services/Images/KickItImageGenerator.ashx?url=http://www.codeassassin.com/blog/PermaLink,guid,9028b79a-a594-48a4-9b70-1b25977f9b6a.aspx)](http://www.dotnetkicks.com/kick/?url=http://www.codeassassin.com/blog/PermaLink,guid,9028b79a-a594-48a4-9b70-1b25977f9b6a.aspx) ]]>
