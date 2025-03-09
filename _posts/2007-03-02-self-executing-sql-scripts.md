---
layout: post
permalink: http://blog.stangroome.com/2007/03/02/self-executing-sql-scripts/
title: Self-executing SQL scripts
description: None
date: 2007-03-01 14:40:29 -0000
last_modified_at: 2012-05-22 03:07:14 -0000
publish: true
pin: false
categories:
- Uncategorized
tags: []
---
I am currently preparing our deployment processes for our most recent software product at work. A fair portion of it involves executing SQL scripts to install and update SQL Server 2005 databases. The deployment will ultimately be performed to several sites by staff who are less familiar with SQL Server than the development team.

At the moment it involves executing the SQLCMD tool from the command line with appropriate parameters to connect to the server, process the script and output a log. However, I feel that this is just one more error-prone step that should be avoided. Some time ago I read about [Polyglots on Wikipedia](http://en.wikipedia.org/wiki/Polyglot_\(computing\))and I was inspired. I thought I would try to write a batch file that also contained a SQL script. The ultimate goal would be a single-file that could be double-clicked and the script would run and the log would be created.

This meant it must be written so the batch command interpreter would ignore the SQL and the SQLCMD tool would ignore the batch commands. The trick was finding the keywords and structures in each language that had similar syntax. After several attempts I settled on GOTO, proving that it isn't totally [harmful](http://www.acm.org/classics/oct95/).

Here is a base example of my solution that you can use to create your own self executing SQL scripts. Just put it in a file with a ".cmd" extension and change as appropriate:

:setvar NUMBEROFROWS 15

GOTO startofpolyglotsqlbatch /* :startofpolyglotsqlbatch @echo off sqlcmd.exe -S MySqlServer -E -e -i "%~f0" -o "%~f0.log" more "%~f0.log" goto endofpolyglotsqlbatch :: */ startofpolyglotsqlbatch:

USE MyDb; GO

SELECT TOP $(NUMBEROFROWS) * FROM MyTable; GO

/* :endofpolyglotsqlbatch :: */

I have added some colour to highlight how the code is interpretted. The initial GOTO is parsed by both the batch command processor and SQLCMD but goes to a different destination for each. The green text is only seen and processed by the batch parser and the blue text is only seen and processed by SQLCMD. The grey text can be replaced with content relevant to your script. This is designed to only work with SQL Server 2005 and only on Windows XP, Windows Server 2003, or later.
