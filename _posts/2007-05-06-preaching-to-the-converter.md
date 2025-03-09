---
layout: post
permalink: http://blog.stangroome.com/?p=117
title: Preaching To The Converter
description: None
date: 2007-05-06 02:42:00 -0000
last_modified_at: 2007-05-06 02:42:00 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

During the past week I have managed to transfer my posts from [my Live Space](http://jasonstangroome.spaces.live.com/) to this new Das Blog site. It was pleasantly trouble free, due mostly to the hard work having been already done by the [BlogML team](http://www.codeplex.com/BlogML/Project/ProjectPeople.aspx).

The biggest headache was the limitation of the [Live Space MetaWeblog API](http://msdn2.microsoft.com/en-us/library/bb259702.aspx) implementation. The API will only allow you to retrieve the most recent 20 posts or, if you know the postId, one post at a time. Unfortunately there is no supported method for retrieving the postIds beyond the most recent 20.

Luckily, upon inspection of the nature of the postIds on my Live Space, and inspection of the [Live Space Team's blog](http://thespacecraft.spaces.live.com/) for confirmation, I discovered a predictable pattern to the postId. It consists of a unique hex string for the space, and an exclamation mark followed by a decimal number that increments with each new post.

With this information I was able to write a Live Space to BlogML converter that, while not exhibiting amazing performance, is able to retrieve all posts on a Live Space much easier than doing it manually or by screen-scraping.

My currently one-way Live Space converter is available in [ChangeSet](http://www.codeplex.com/BlogML/SourceControl/ListDownloadableCommits.aspx) 21935 and later of the BlogML project on CodePlex. Hopefully the project coordinators will build a new release soon and it will then be available as part of the main package.

To use the converter, start by [enabling email publishing](http://msdn2.microsoft.com/en-us/library/bb259698.aspx) on your Live Space. Then create a new Visual Studio project and reference the LiveSpace.BlogML assembly. Construct an instance of the LiveSpaceBlogMLWriter passing the name of your Live Space and your email publishing secret word. Optionally set the PostCount property to a number ideally no greater than the number of posts on your site. Finally, call the Write method passing a preconfigured XmlWriter as the destination.

Please submit any issues you have with the converter to the [BlogML Discussion](http://www.codeplex.com/BlogML/Thread/List.aspx) pages and I'll endeavour to solve them. For those without the resources or inclination to write a program as mentioned above, let me know and I'll find some time to create a user interface for it.

UPDATE: [Doron Yaacoby has created a GUI for the Live Space BlogML converter](http://blogs.microsoft.co.il/blogs/dorony/archive/2007/08/11/Converting-a-Windows-Live-Spaces-blog-to-BlogML.aspx).

]]>
