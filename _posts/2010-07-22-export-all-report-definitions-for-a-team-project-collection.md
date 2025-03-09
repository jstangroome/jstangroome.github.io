---
layout: post
permalink: http://blog.stangroome.com/2010/07/22/export-all-report-definitions-for-a-team-project-collection/
title: Export all report definitions for a Team Project Collection
description: None
date: 2010-07-22 08:05:41 -0000
last_modified_at: 2010-07-22 08:05:41 -0000
publish: true
pin: false
categories:
- PowerShell
- SQL Server
- TFS
tags: []
---
Team Foundation Server 2010 introduced Team Project Collections for organising Team Projects into groups. Collections also provide a self-contained unit for moving Team Projects between servers and this is [well documented and supported](http://msdn.microsoft.com/en-us/library/dd936138.aspx). However, if you've ever tried moving a Team Project Collection you'll find the documentation is a long list of manual steps, and one of the more tedious steps is [Savings Reports](http://msdn.microsoft.com/en-us/library/dd936138.aspx#SaveReports). This step basically tells you to use the Report Manager web interface to manually save every report for every Team Project in the collection as an .RDL file. A single project based on the MSF for Agile template will contain 15 reports across 5 folders, so you can easily spend a while clicking away in your browser. To alleviate the pain, I've written a PowerShell script which accepts two parameters. The first is the url for the Team Project Collection, and the second is the destination path to save the .RDL files to. The script will query the Team Project Collection for its Report Server url and list of Team Projects via the TFS API, then it will use the Report Server web services to download the report definitions to the destination, maintaining the folder hierarchy and time stamps. You can access this script, called Export-TfsCollectionReports, [on Gist](http://gist.github.com/485519). Obviously, when you reach [the step to import the report definitions](http://msdn.microsoft.com/en-us/library/dd936138.aspx#Reports) on the new server, you'll want a similar script to help. Unfortunately, I haven't written that one yet but I will post it to my blog when I do. In the mean time you could follow the same concepts used in the export script to write one yourself.
