---
permalink: http://blog.stangroome.com/2007/10/13/find-duplicate-files-with-powershell/
title: Find Duplicate Files With PowerShell
description: None
date: 2007-10-13 03:27:20 -0000
last_modified_at: 2012-04-03 00:29:36 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
I have pieced together a simple PowerShell script to recursively locate all duplicate files (by content, not name) below a chosen directory. It is not the most elegant code but for my purposes it works and hopefully you will be able to tweak it to suit your needs. Firstly, it filters out any zero-length files. Zero-length files are naturally duplicates of each other and can be found quite trivially without my script. Secondly it groups all files by their length because if the length doesn't match, they can't have the same content. The script then excludes the length-groups with only one entry and calculates the MD5 hash of the remaining files. Groups of files with both matching size and hash are then returned in the results. The hashing function was taken from the [Duplicate Files](http://blogs.msdn.com/powershell/archive/2006/04/25/583225.aspx) post on the Windows PowerShell team blog. It simply uses the .NET cryptography namespace to compute the hash. From here you could easily exchange the MD5 algorithm for SHA1 or any other preferred algorithm. Due to the need to read the entire contents of potentially matching files to compute the hash this can cause the script to take a long time against larger files. Executing the script against deep directory structures with many files will take longer too. The script could be easily modified to take a filtered input of files to only find, for example, duplicate photos. Update: The [script is now available on github:gist](https://gist.github.com/2288218). You should save it as "Get-DuplicateItems.ps1". Once you have the output of the script you could use it delete the unnecessary files:
  
    $dupes = .\Get-DuplicateItems.ps1
    $dupes | % { ($null, $rest) = $_.Group; $rest } | Remove-Item -WhatIf

As always, if you have any suggestions or improvements don't hesitate to contact me.
