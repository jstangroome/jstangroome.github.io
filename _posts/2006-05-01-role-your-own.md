---
layout: post
permalink: http://blog.stangroome.com/?p=128
title: Role your own
description: None
date: 2006-04-30 16:39:26 -0000
last_modified_at: 2006-04-30 16:39:26 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I discovered an annoying limitation in the SQL Server Management Studio today. The GUI does not offer an option to add a Database Role as a member of another Database Role. In my case I wanted to add my "bigAppAdmins" role to the "bigAppUsers" role so all administrators are implicitly users also. Unfortunately I had to resort to the [sp_addrolemember](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/tsqlref/ts_sp_addp_4boy.asp) stored procedure to achieve this. If I didn't already know that roles could be members of other roles I might have been convinced that SQL Server didn't support it.
