---
layout: post
permalink: http://blog.stangroome.com/?p=113
title: DataSet Diagrams
description: None
date: 2007-05-27 12:36:54 -0000
last_modified_at: 2007-05-27 12:36:54 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

We often use Strongly-Typed DataSets in our applications at work. They can be a useful tool but they have their limitations too. Today, I'm not going to discuss the pros and cons of DataSets versus some other DAL concept. Today I'm just writing about a useful tip for working with growing DataSets.

As changes are made to tables and relationships in the DataSet Designer, eventually all the diagrams become unordered and relationship links begin to wander like vines. Unfortunately, I have never been able to find in Visual Studio an option to re-layout the diagram automatically for optimum table sizes and minimum relationship intersections. You don't have to shuffle them around manually though, there is another way:

  1. Open the project containing the DataSet and Show All Files in the Solution Explorer.
  2. Ensure the DataSet designer windows are closed.
  3. Open the DataSet's .XSS file in a code window (it should be XML with <Shape> elements among others).
  4. Clear the entire contents of the XSS file, leaving it blank.
  5. Save the XSS file and close the window.
  6. Open the DataSet's designer window, you should find all the tables and relationships nicely arranged.

It's been a busy day but I should have another overly long post ready in time for next week.

]]>
