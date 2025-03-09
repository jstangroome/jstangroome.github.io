---
layout: post
permalink: http://blog.stangroome.com/?p=83
title: Dreams and Mistakes
description: None
date: 2007-07-02 13:21:19 -0000
last_modified_at: 2007-07-02 13:21:19 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Firstly, in [my recent post](http://www.codeassassin.com/blog/PermaLink,guid,9a2ef27f-be0f-4545-a864-521c5d1e5d6b.aspx) about the new .Net [DateTimeOffset](http://msdn2.microsoft.com/en-us/library/system.datetimeoffset\(vs.90\).aspx) class I mentioned that I would love to see the SQL Server team include this type in databases. My wishes [have been answered](http://dotnetsamplechapters.blogspot.com/2007/06/sql-server-2008-will-have-7-new.html), we can expect to see this type and some other very interesting additions in the next CTP/Beta of SQL Server 2008. Unfortunately, all of our clients have bought into SQL Server 2005 and won't be interested in upgrading anytime soon so I'll have to find a new project to try all the new toys.

Secondly, also in [a recent post](http://www.codeassassin.com/blog/PermaLink,guid,338eb27b-7af2-4229-9668-6f021e1f3f95.aspx) I complained about the [CollectionBase](http://msdn2.microsoft.com/en-us/library/system.collections.collectionbase.aspx) type being left behind when generics were added to .NET 2.0. I've since discovered the new [Collection<T>](http://msdn2.microsoft.com/en-us/library/ms132398\(VS.80\).aspx) class tucked away in the [System.Collections.ObjectModel](http://msdn2.microsoft.com/en-us/library/ms132396\(VS.80\).aspx) namespace. It doesn't offer all the events that the previous equivalent did but it is inheritable and overridable so it should do the job nicely. I obviously didn't search very hard for it all the other times I looked; very embarrassing.
