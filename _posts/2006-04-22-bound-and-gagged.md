---
layout: post
permalink: http://blog.stangroome.com/?p=133
title: Bound and gagged
description: None
date: 2006-04-21 22:55:06 -0000
last_modified_at: 2006-04-21 22:55:06 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[I haven't used much of the built in data binding in .NET yet, preferring to keep things under my own control. However, the current project is using it heavily and I really should get up to speed before it's too late. Here is a small tip that was easy to miss on some of the project's earlier code:  
  
"Try using myBindingSource.ResetCurrentItem instead of myBindingSource.ResetBindings when you need to propagate small data changes throughout an application. It can be much more efficient when you are dealing with big lists."  

]]>
