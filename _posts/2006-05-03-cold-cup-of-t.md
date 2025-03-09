---
layout: post
permalink: http://blog.stangroome.com/?p=126
title: Cold Cup(Of T)
description: None
date: 2006-05-02 15:13:21 -0000
last_modified_at: 2010-07-20 00:10:48 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I often wonder exactly which collection interface I should implement in a new collection class or which I should use as a method parameter. I like to choose the highest level interface that will provide access to the properties and methods I need. Instead of following the links through MSDN Library everytime I quickly [created a map](http://www.jsolutions.com.au/public/CollectionInterfaceMap.pdf) in Visio so I can see all the relationships between the standard framework collection interfaces and classes, including the new generic items. It probably isn't complete but I think it is good guide for getting information at a glance.

It was interesting to discover the significant differences between the generic and non-generic ICollection interfaces. The original non-generic ICollection provides enumeration support and a Count property. The new generic ICollection(Of T) goes further and includes support for adding and removing items, much more like IList than ICollection. It doesn't even inherit from ICollection which unfortunately means that while most of the standard implementation classes of ICollection(Of T) also implement ICollection, I can't safely pass ICollection(Of T) objects to AddRange methods or common collection constructors.
