---
layout: post
permalink: http://blog.stangroome.com/2012/04/10/test-attachment-cleaner-least-privilege/
title: Test Attachment Cleaner Least Privilege
description: None
date: 2012-04-10 04:53:29 -0000
last_modified_at: 2012-04-10 04:53:29 -0000
publish: true
pin: false
categories:
- PowerShell
- TFS
tags: []
---
I manage a Team Foundation Server 2010 instance with at least 30 Collections each with several Team Projects. Even after installing the [TFS hotfix to reduce the size of test data](http://support.microsoft.com/kb/2608743) in the TFS databases we still accrue many binary files care of running Code Coverage analysis during our continuous integration builds. As such it is still necessary to run the [Test Attachment Cleaner](http://blogs.msdn.com/b/granth/archive/2011/02/12/tfs2010-test-attachment-cleaner-and-why-you-should-be-using-it.aspx) Power Tool regularly to keep the database sizes manageable. The Test Attachment Cleaner however is a command-line tool which requires the Collection Uri and the Team Project Name among several other parameters so I first needed to write a script (I chose PowerShell) to query TFS for all the Collections and Projects and call the Cleaner for each. I then needed to configure this script (and therefore the Cleaner) to run as a scheduled task and I needed to specify which user the task would run as. The easiest answer would be to run the task as a user who has TFS Server Administrator privileges to ensure the Cleaner has access to find and delete attachments in every project in every collection but that would be overkill. I couldn't find any existing documentation on the minimum privileges required by the Cleaner so I started with a user with zero TFS privileges and repeatedly executed the scheduled task, granting each permission mentioned in each successive error message until the task completed successfully. For deleting attachments by extension and age I found the minimum permissions required were the following three, all at the Team Project level:
    * View test runs
    * Create test runs (non-intuitively, this permission allows attachments to be deleted)
    * View project-level information
For deleting attachments based on linked bugs (something I didn't try) I suspect the "View work items in this node" permission would also be required at the root Area level. Having determined these permissions, I needed to apply them across all the Team Projects but it appears the only out-of-the-box way to set this permissions is via the Team Explorer user interface which becomes rather tedious after the first few projects. Instead I scripted the permission granting too via the TFS API and I've [published some PowerShell cmdlets](https://github.com/codeassassin/pstfs2010) to make this easier if anyone else needs to do the same.
