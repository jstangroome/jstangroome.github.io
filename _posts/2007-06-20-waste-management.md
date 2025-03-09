---
layout: post
permalink: http://blog.stangroome.com/?p=93
title: Waste Management
description: None
date: 2007-06-20 12:40:32 -0000
last_modified_at: 2007-06-20 12:40:32 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

The script in [my last post](http://www.codeassassin.com/blog/PermaLink,guid,d6ff9a43-6439-4acf-af45-12ee3089cf7d.aspx) for removing old files from the temporary folder is only necessary because so many applications don't clean up after themselves. Hopefully, if the applications you are developing are doing the right thing they will be using in memory streams where ever possible. Only write to disk when absolutely necessary, creating random temporary files in the environment temp folder, not program folders with restricted access to non-admin users:

string myTempFile = System.IO.Path.GetTempFileName();  
  
However, remembering to track and delete these files when you are finished requires more effort and is often ignored, creating the ever growing temp folder problem. There is a lesser known class in the framework that can help you though:

var myTempColl = new System.CodeDom.Compiler.TempFileCollection();  
string myTempImageFile = myTempColl.AddExtension("png");  
  
Excuse the C# 3.0 syntax in the last snippet, I hate the redundancy of repeating long type names, especially on the same line. When you have finished using all the temp files you created with the TempFileCollection, you can dispose it explicitly, automatically deleting the files, or if you're really lazy, let the GC take care of it.

myTempColl.Dispose();

]]>
