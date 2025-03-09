---
layout: post
permalink: http://blog.stangroome.com/?p=87
title: CollectionBase and Generics
description: None
date: 2007-06-26 12:57:19 -0000
last_modified_at: 2007-06-26 12:57:19 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

In .NET 1.1 the Framework included an abstract class [System.Collections.CollectionBase](http://msdn2.microsoft.com/en-us/library/system.collections.collectionbase.aspx). The idea was that you could extend it to create your own custom collections to hold certain types. The Add, Remove, and other methods were overridable (to perform object validation or filter duplicates) and several events also existed to enable listening for changes.

[![Cardboard Box](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/CollectionBaseandGenerics_133DA/cardboardbox_1.jpg)](http://www.flickr.com/photos/ahhyeah/454494396/) Most importantly it provided the mind-numbing mapping from the IList, ICollection and IEnumerable interfaces to the internal storage mechanism (which happened to be an ArrayList). When I created a new collection derived from CollectionBase I could [override the OnValidate method](http://www.mattberther.com/?p=540) to only accept a certain type and I was done.

Sadly, I'm left stranded in my .NET 2.0 world with Generics and an untyped CollectionBase. I cannot inherit from List<T> because none of it's methods are virtual and none of it's fields are protected. Ultimately, I'm stuck with writing my own class with a private List<T> field and coding most of the methods on the various interfaces as redirections to the private field. No thanks, that was one of the biggest reasons for moving from VB6 to .NET.

Interestingly, the SharePoint fellows noticed the same problem with CollectionBase's obsolescence in .NET 2.0, and cleverly decided to provide the more up-to-date [CollectionBase<T>](https://msdn2.microsoft.com/en-us/library/ms567715.aspx). Of course, not only did it end up in the Microsoft.SharePoint.Publishing assembly that I'm reluctant to reference and probably unable to distribute, but they also implemented only ICollection and IEnumerable<T>. I was dumbfounded that IList<T> and ICollection<T> had been missed and that they had also removed IList.

Why am I still forced to code this junk myself?

]]>
