---
layout: post
permalink: http://blog.stangroome.com/?p=86
title: DateTimeOffset Is The New DateTime
description: None
date: 2007-06-26 13:16:16 -0000
last_modified_at: 2007-06-26 13:16:16 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

[![Sundial](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/DateTimeOffsetIsTheNewDateTime_13C70/sundial_1.jpg)](http://www.flickr.com/photos/jillgood/47241060/)[Justin Van Patten neatly summarises](http://blogs.msdn.com/bclteam/archive/2007/06/14/datetimeoffset-a-new-datetime-structure-in-net-3-5-justin-van-patten.aspx) the new [DateTimeOffset](http://msdn2.microsoft.com/en-us/library/system.datetimeoffset\(vs.90\).aspx) class introduced in .NET 3.5. In short, this new class combines the year, month, blah, blah, second storage of the DateTime with a time zone offset. The most important point to take from Justin's article is that apart from a few exceptions (which he describes), you should always use DateTimeOffset in new code instead of the old DateTime.

Naturally there will continue to be situations where you need to pass a DateTime but the DateTimeOffset will always be able to scale back to suit. It is much harder to infer time zone information from a DateTime to provide a DateTimeOffset. I'd love to see the SQL Server team introduce this data type concept to databases.

]]>
