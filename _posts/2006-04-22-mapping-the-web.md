---
layout: post
permalink: http://blog.stangroome.com/?p=132
title: Mapping the web
description: None
date: 2006-04-21 23:00:48 -0000
last_modified_at: 2006-04-21 23:00:48 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[Discovered recently that Windows XP has really good support for WebDAV. This command will map an unused drive letter to a folder shared via HTTP:  
  
net use * http://webdavserver/webdavfolder  

It's that easy. Hosting a WebDAV folder is also really easy on any machine with IIS. Just add a virtual directory, point it to your chosen local path and make sure it is browsable.  
]]>
