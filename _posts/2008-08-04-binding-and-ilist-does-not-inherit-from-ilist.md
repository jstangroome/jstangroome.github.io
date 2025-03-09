---
layout: post
permalink: http://blog.stangroome.com/2008/08/04/binding-and-ilist-does-not-inherit-from-ilist/
title: Binding... and generic IList does not inherit from IList.
description: None
date: 2008-08-04 07:05:34 -0000
last_modified_at: 2012-02-20 22:17:14 -0000
publish: true
pin: false
categories:
- Dot Net
tags: []
---
A note to myself, and to others who may find it useful: Use [System.Collections.ObjectModel.Collection<T>](http://msdn.microsoft.com/en-us/library/ms132397.aspx) (or a subclass) as the return type for properties exposing collections of related items on your presentation model classes if you intend to bind to the relationships with Windows Forms [BindingSources](http://msdn.microsoft.com/en-us/library/system.windows.forms.bindingsource.aspx). Collection<T> appears to be the lowest-level type in Framework 3.5 that provides both the non-generic IList implementation as required by the binding system and also provides strongly-typed programmatic access to the elements of the collection. It is used as the base for BindingList<T>, ObservableCollection<T> and many others and you can easily inherit from it yourself. Using a return type of IList<T> or ICollection<T> instead, which may be preferred for being less concrete, is **not** sufficient as neither of these inherit from the non-generic IList and will fail to bind correctly (sometimes throwing exceptions) at run-time when the parent list of the bound relationship is empty.
