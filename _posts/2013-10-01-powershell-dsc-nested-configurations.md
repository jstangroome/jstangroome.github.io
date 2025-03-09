---
layout: post
permalink: http://blog.stangroome.com/2013/10/01/powershell-dsc-nested-configurations/
title: PowerShell Desired State Configuration Nested Configurations
description: None
date: 2013-10-01 09:34:03 -0000
last_modified_at: 2013-10-01 09:34:03 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
In PowerShell v4's new [Desired State Configuration](http://technet.microsoft.com/en-us/library/dn249912.aspx) feature, each Node¹ defined within a Configuration results in a single MOF file and each computer's [Local Configuration Manager](http://technet.microsoft.com/en-us/library/dn249922.aspx) will only apply and monitor configuration from a single MOF file at a time. This might suggest that all the complexity of the [Resource](http://technet.microsoft.com/en-us/library/dn282125.aspx) configuration for an entire computer needs to be captured within the a single Node and that DSC configuration files will become big and unwieldy fast. However, I recently have noticed that the Get-DscResource cmdlet returns not only the built-in and custom resources installed on the computer but it also lists any Configurations defined in the current session. From this I inferred that it may be possible to include one Configuration within another so that the overall configuration of a system can be modularized into more manageable pieces. I had an opportunity to raise this with a member of the PowerShell team and it was clarified for me that this is indeed possible, and presumably recommended. The key is that the resulting combined configuration definition must only contain one level of Nodes. That is, either the parent Configuration defines the Node, or the child Configuration defines the Node, not both. Here is an example of defining two child Configurations which combine related Resources and a parent Configuration which then defines a Node containing both of these Configurations: https://gist.github.com/jstangroome/6775904 A name still needs to be associated with the child Configurations when they are included in a parent, but they may or may not require additional parameters. There is no reason that the nested Configurations be limited to any particular resource, I've just used the WindowsFeature resource in this example to avoid external dependencies. ¹Subject to the use of [Configuration Data](http://technet.microsoft.com/en-us/library/dn249925.aspx) which may repeat a single Node multiple times.
