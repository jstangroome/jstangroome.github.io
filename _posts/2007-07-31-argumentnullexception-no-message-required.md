---
layout: post
permalink: http://blog.stangroome.com/?p=67
title: 'ArgumentNullException: No Message Required'
description: None
date: 2007-07-31 11:26:53 -0000
last_modified_at: 2007-07-31 11:26:53 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I have been encountering code recently that bugs me. Many methods in classes are testing their arguments for null/Nothing and, if true, throwing a new ArgumentNullException. So far, all good. However, the constructor for the new ArgumentNullException is being passed both the name of the argument and also a message string of the form "_paramName_ cannot be null".

This is totally redundant. The exception being thrown is an Argument *Null* Exception. That about sums it up. Also, if the constructor is passed the parameter name only the message defaults to "Value cannot be null".

Providing a message for an ArgumentNullException requires extra typing, requires maintaining the argument name in three places if it gets renames, and violates FxCop rule [CA1303](http://msdn2.microsoft.com/en-us/library/ms182187\(VS.80\).aspx).

Thankfully it turns out it isn't fellow team members doing it. [Developer Express' Refactor Pro](http://www.devexpress.com/Products/NET/IDETools/Refactor/) has a "Create Method Contract" refactoring that is putting the message in.
