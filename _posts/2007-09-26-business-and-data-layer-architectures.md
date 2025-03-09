---
layout: post
permalink: http://blog.stangroome.com/?p=52
title: Business and Data Layer Architectures
description: None
date: 2007-09-26 13:05:46 -0000
last_modified_at: 2007-09-26 13:05:46 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Strongly Typed DataSets in .NET 2.0 do many things very well but they fall short in other places. For a new project I have been researching alternative approaches for structuring the model, logic, and data access projects to allow for easy customisation and good testing.

Firstly, there are some features about DataSets that are very useful and would need to exist in any alternative I consider:

* Collection filtering and sorting by any properties.
* Collection and item change notification.
* IEditableObject support (ie BeginEdit, EndEdit, CancelEdit).
* Excellent encapsulation of SQL commands.

There is also the limitations of DataSets that should not exist in the alternative I choose:

* Computed values too complex for DataColumn.Expression cannot be bound to.
* AllowDBNull and MaxLength violations cause immediate exceptions.
* Restrictive relationship between model and underlying database structure.
* Does not utilise Nullable<T> to support columns that allow DBNull.
* Change notification events cannot be completely silenced.

So far I have briefly investigated [CSLA](http://www.lhotka.net/cslanet/), [SubSonic](http://www.subsonicproject.com/), [NHibernate](http://www.hibernate.org/343.html), and some existing applications both real ([DotNetKicks](http://code.google.com/p/dotnetkicks/)) and example ([MS Pet Shop](http://msdn2.microsoft.com/en-us/library/aa479071.aspx)), and found that they are all very similar in many ways. Rather than commit myself to a particular framework that may have it's own set of issues I have decided to hand code my own solution to the business and data layering problem.

[Others](http://notgartner.wordpress.com/2007/09/25/frameworks-frameworks-frameworks/) have expressed concerns about going with a third party framework and to a point I agree, but I'm not totally sold either way yet.

I intend to watch my own code evolve as I solve the difficulties associated with the task and refactor my work into common classes. I expect that my code will begin to look much like one of the existing frameworks (although that may be biased by what I've already seen) and perhaps I will end up switching to the solution that most closely matches my own.

The last thing we need is yet another framework.
