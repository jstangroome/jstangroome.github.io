---
layout: post
permalink: http://blog.stangroome.com/2007/09/28/streaming-large-objects-with-ado-net-properly/
title: Streaming Large Objects with ADO.NET, Properly
description: None
date: 2007-09-27 14:42:27 -0000
last_modified_at: 2010-07-20 00:07:07 -0000
publish: true
pin: false
categories:
- Dot Net
- SQL Server
tags: []
---
It has been a while since I last worked with storing files in a SQL database and I decided to Google around to remind myself of the best way to do it. I was very disappointed with most of the approaches I found. Unfortunately, my Google-Fu didn't return the MSDN articles I've linked to below, and I had to find out the hard way. To begin, all solutions I found dealt only with reading a BLOB from a SQL Server image or varbinary(max) column in a streaming fashion. Worst of all very few actually understood what streaming should do, and that is not load the entire object into an array in memory. My whinging aside, streaming a file out of a SQL table is easy. You start by using a DataReader created by passing CommandBehavior.SequentialAccess to a DbCommand's ExecuteReader function. I also find that selecting only the blob column and only the desired row(s) from the table is the most effective. When you have the DataReader positioned on the appropriate record you repeatedly call the GetBytes method in a loop, retrieving a small chunk each time and writing it to the output stream. The output can be any IO.Stream like a file or even your ASP.NET response. [This MSDN article](http://msdn2.microsoft.com/en-us/library/87z0hy49\(vs.80\).aspx) has a good description of the situation with the SequentialAccess enumeration and some sample code. Writing a stream of data into a SQL table turned out to be slightly less obvious. I'm only working with SQL Server 2005 so I didn't consider supporting older versions but the approach is similar. SQL 2005 provides a Write "method" on the large value data types in the [UPDATE](http://msdn2.microsoft.com/en-us/library/ms177523.aspx) statement. My solution was to first insert the new row into the table providing values for all columns except the blob. Then I had a stored procedure that would take the row's primary key values, an offset, and a chunk of the data to insert and use the UPDATE .Write method to update the row. Similar to the reading code, my writing code would read a small chunk from the incoming IO.Stream and pass it to the stored procedure, incrementing the offset each time. Once again, there is [another MSDN article](http://msdn2.microsoft.com/en-us/library/3517w44b\(VS.80\).aspx) that describes the process well but their code looks like it will also work with SQL versions prior to 2005. In both cases tweaking the size of the chunk used in each iteration of the loop will require some testing and measuring to find the best performance but now you can read and write files of almost 2GB into SQL Server without trying to allocate a similarly sized array in memory first.
