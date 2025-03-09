---
layout: post
permalink: http://blog.stangroome.com/2012/05/29/threat-management-gateway-host-header-forwarding-and-redirection/
title: Threat Management Gateway, Host header forwarding and redirection
description: None
date: 2012-05-29 03:59:05 -0000
last_modified_at: 2012-05-29 03:59:05 -0000
publish: true
pin: false
categories:
- Uncategorized
tags: []
---
When using Forefront Threat Management Gateway 2010 (TMG) to expose an internal web server to the public Internet beware of the unexpected side-effects of the "Forward the original host header instead of the actual one (specified in the Internal site name field)" check box on the "To" tab in the properties dialog of a Web Publishing Rule. If the behaviour of the internal web server is to detect an incoming request over HTTP and respond with a [3xx response](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes#3xx_Redirection) to a new **HTTPS** location (for all or only specific URLs) **and** the "Forward the original host header" option in TMG is checked, then TMG interferes and the HTTP Location header that is returned to the original client does not include the HTTPS scheme and the client's browser/user-agent gets caught in an [infinite redirection loop](http://en.wikipedia.org/wiki/URL_redirection#Redirect_loops). [![](http://blog.stangroome.com/wp-content/uploads/2012/05/forwardhost.png)](http://blog.stangroome.com/wp-content/uploads/2012/05/forwardhost.png) Unchecking the "Forward the original host header" option for the rule and applying the changes fixes the issue and the redirection works correctly. I have been unable to find any official documentation on this behaviour and only two references to anything related. I found [one post](http://forums.isaserver.org/m_2002034241/mpage_1/key_/tm.htm) on the [isaserver.org](http://www.isaserver.org/) forums (with no responses) suggesting that this behaviour was introduced in ISA 2004 SP2 (ISA being the original name for TMG). The other reference is a [Microsoft Support KB article](http://support.microsoft.com/kb/925287) describing that the request Host header passed from TMG to the internal web server can include the port number with the host name - if the port number is embedded I can imagine this impacting the scheme, but it's a stretch. Hopefully this post will help the next person hitting the same issue.
