---
layout: post
permalink: http://blog.stangroome.com/?p=76
title: 'Code Camp SA: Michael Baker'
description: None
date: 2007-07-08 09:29:24 -0000
last_modified_at: 2007-07-08 09:29:24 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Michael Baker presented a session entitled "Design By Contract" on Saturday morning. [Design by contract](http://en.wikipedia.org/wiki/Design_by_contract) is a broad idea that can be approached in several ways. Michael focused on the idea of using pre- and post-condition macros in all your functions and combining that with extensive testing to ensure the conditions are met at runtime on all code paths. He also focused on implementing the macros as assertions rather than error return codes or exceptions.

I think that design by contract should not be treated so specifically but more as a combination of both discipline and an effective implementation for early problem detection.

You may establish a rule to document all methods with a comment header describing the intentions and expectations or you may insist (like [FxCop](http://msdn2.microsoft.com/en-us/library/ms182182\(vs.80\).aspx)) on throwing exceptions for invalid parameters. The important point is that it is done consistently.

As for effective implementation, comments are great for readability and guard clauses or unit tests are great for automated checking. However, the best outcome will be achieved with a system that can verify _at compile time_ that all code paths pass the conditions and perhaps autogenerate human readable documentation too.

Defining the type of method parameters and return values in C# and VB is already a basic form of compile time verification of very basic conditions. Microsoft Research offers a variant of C#, known as [Spec#](http://research.microsoft.com/specsharp/). It extends on the C# language using new keywords and a variety of custom attributes to enable the developer to specify, declaratively, what values are valid as inputs and outputs from methods. The Spec# compiler is then able to use basic static analysis technique during build to highlight problem errors of code that could pass unacceptable values.

However, this all falls down when there are environmental conditions that cannot be verified at compile time. The absence of an expected file could throw an exceptions at runtime and the availability of network resources is a common problem also. Both Java and Spec# help here with checked exceptions and [more advanced static analysis tools](http://wesnerm.blogs.com/net_undocumented/2005/12/nstatic_and_exc.html) can track where possible exceptions can be thrown and verify that the application handles them appropriately.
