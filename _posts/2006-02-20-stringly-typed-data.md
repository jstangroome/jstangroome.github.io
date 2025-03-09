---
layout: post
permalink: http://blog.stangroome.com/?p=136
title: Stringly Typed Data
description: None
date: 2006-02-20 02:35:14 -0000
last_modified_at: 2006-02-20 02:35:14 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
The project I am currently working on involves the new SQL Server 2005 Reporting Services. So far, other members of my team have been responsible for the reporting modules but I know I will be working with it soon so I decided to do some preliminary research.  
  
I believe in keeping my blog as a source of positive information and I prefer to post about problems where I have already found a solution I can offer to the community. However, a particular problem I have encountered with Reporting Services does not seem to have a solution, so I hope this post will bring more attention to the problem and perhaps a solution will eventually be provided by Microsoft. This is assuming that this problem is not a result of my inability to find the right documentation.  
  
I am referring to the ReportParameter class in the Microsoft.Reporting.WinForms namespace. This class is used to pass parameters to the Report Server for determining the contents of the report produced. The problem is that the ReportParameter only seems to support String typed parameters. Considering many reports will be based on the results returned by stored procedures in SQL Server I would expect Reporting Services to be using a very similar structure for its input parameters.  
  
With the current structure I cannot pass NULL to a ReportParameter. I would need to use special values such as -1 maybe for an int parameter and the stored procedure would need to be changed to understand that. If I want to be able to distinguish between an empty string and a NULL string I have to go to more effort to choose a special value that won’t be used in my data.  
  
Another filter that I will commonly use for reports is a date filter. The user should be able to choose a start and end date and generate a report with data within that range. With standard Stored Procedure calling, ADO.NET will handle all the necessary converions for passing DateTime parameters in the correct format for the database. With Reporting Services I need to convert the DateTime to a String manually by considering the locale of the user’s computer and the Report Server.  
  
ADO.NET has been built to provide a base level of data classes to suit all types of features and capabilities found in various database engines. There is no reason why Reporting Services should not have been developed the same way, especially considering this is now the second release of the product.  
  
I am interested in reading the comments of others on this topic and if I am wrong about all this can someone please point me in right direction.  
  