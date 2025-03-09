---
layout: post
permalink: http://blog.stangroome.com/?p=123
title: What rolls down stairs, alone or in pairs?
description: None
date: 2007-02-24 03:06:57 -0000
last_modified_at: 2007-02-24 03:06:57 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[![](http://www.codeassassin.com/blog/content/binary/blamolog.jpg)

I recently discovered that our databases in SQL Server 2005 using the Bulk Logged recovery model were not getting their logs backed up by the Log Backup maintenance plan. I did some searching to no avail and ending up posting a question to the MSDN forums. I was told this is a known bug in SQL 2005 RTM and SP1 and will be fixed in SP2. SP2 is now available and the problem is fixed... but not very well.

 

The Log Backup maintenance plan allows you to choose "all databases" when asked what to back up. In pre-SP2 it would select all databases on the server, filter out the ones with inappropriate recovery models, then execute a BACKUP LOG statement against the remaining databases. The problem was that instead of only excluding Simple recovery models databases, it excluded Bulk Logged also. I imagined the fix in SP2 would be to correct the filter. Not so.

 

Now, in SP2, the maintenance plan selects all databases on the server and attempts to execute a BACKUP LOG statement against each of them. Those with appropriate recovery models succeed, the others fail. Unfortunately the failures are then treated by SQL Agent as a job error and the maintenance plan reports as failed even though all the right databases were backed up successfully.

 

The benefit in SP2 is that the Bulk Logged databases actually get backed up now because you couldn't back them up at all with a maintenance plan previously even if you explicitly selected the Bulk Logged databases only. The downside is that I have to update my maintence plan everytime a new database is added or the recovery model changes. I appreciate that you should give more time to getting backups right on a production database, but on the development servers this is tedious.

]]>
