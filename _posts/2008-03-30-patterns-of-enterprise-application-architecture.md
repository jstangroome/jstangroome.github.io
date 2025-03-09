---
layout: post
permalink: http://blog.stangroome.com/?p=32
title: Patterns Of Enterprise Application Architecture
description: None
date: 2008-03-30 06:23:10 -0000
last_modified_at: 2008-03-30 06:23:10 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

I just finished reading [Martin Fowler's](http://martinfowler.com/) book, [P of EAA](http://www.amazon.com/gp/redirect.html?ie=UTF8&location=http%3A%2F%2Fwww.amazon.com%2FEnterprise-Application-Architecture-Addison-Wesley-Signature%2Fdp%2F0321127420%2F&tag=codeassa-20&linkCode=ur2&camp=1789&creative=9325)![](http://www.assoc-amazon.com/e/ir?t=codeassa-20&l=ur2&o=1). Fowler does a great job of presenting various ways to approach solving each given enterprise architecture problem in an object-oriented way and explains the circumstances that will suit each solution and where each solution starts to fall down. This is a welcome change from books and blogs that preach The One True Way.

As [Ayende](http://ayende.com/) has [blogged recently](http://ayende.com/Blog/archive/2008/03/15/Recommended-Books.aspx), the patterns described in the book made a lot of sense of code like that in the NHibernate framework and at the same time convinced him that there is no real reason to re-implement those patterns. While I agree that implementing all the required structures yourself is both a large task and a good reason to "buy" an existing system, learning to code something like this yourself can really help to improve your understanding.

Fowler also opened my eyes as to how various classes in the .NET Framework relate to these patterns, and therefore, what methods for interacting with such classes work well. One example, is how ADO.NET's DataSet compares to the [Unit Of Work](http://martinfowler.com/eaaCatalog/unitOfWork.html). Rather than having a DataSet fill from a database, then exist for the life of an application and commit back to the database at the end, instead the life of a DataSet should correspond closely to that of a particular business transaction.

The book also contains a good mix of both Java and C# examples, and highlights how, with different built-in tools in these two development environments, each of these languages implements a pattern with its own strengths. My only concern was that much of the discussion leaned toward web application scenarios. My preferred domain, smart client applications, have a different set of issues to solve but admittedly the book is about architecture patterns, not about smart client presentation patterns, so I can't complain.

I can recommend this book as a good read for anyone writing large business applications.

]]>
