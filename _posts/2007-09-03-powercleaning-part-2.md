---
layout: post
permalink: http://blog.stangroome.com/2007/09/03/powercleaning-part-2/
title: PowerCleaning, Part 2
description: None
date: 2007-09-03 12:50:38 -0000
last_modified_at: 2010-07-18 22:32:55 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
Each day I live with wanting to make full use of excess storage space to keep a record of every file that has ever existed in all it's versions and the conflicting need to have a neat and tidy file system without any junk sitting around. Recently, the desire to have a ClickOnce deployment folder with only the latest version in it has won. With only the most recently published version within, the folder is a much more manageable size for distributing to other deployment servers. However, it can be tedious to ensure only the appropriate files are deleted when cleaning a deployment folder manually, and automating the process is infinitely better. I have put together a basic PowerShell function to handle the situation. function Clean-ClickOnceApplication ([string] $Path = (Get-Location), [switch] $WhatIf = $false) { ($current, $oldList) = Get-ChildItem -path (Join-Path $Path "*") -include "*.application" ` | ? {$_.Name -match "_\d+_\d+_\d+_\d+\\." } | sort LastWriteTime -desc; foreach ($old in $oldList) { $oldFolder = $old.Name.Substring(0, $old.Name.Length - $old.Extension.Length); Remove-Item (Join-Path $Path $oldFolder) -recurse -WhatIf:$WhatIf; Remove-Item $old -WhatIf:$WhatIf; } }  Simply pass the ClickOnce deployment folder as the -Path parameter (or omit to assume the current folder) and optionally enable the -WhatIf switch to test which files and folders would have been deleted. The script currently uses the time stamps of the .application files rather than the version numbers but unless someone has been messing with the digitally-signed files this shouldn't be a problem. If it really needs to be handled by version number, I have some ideas for handling that.
