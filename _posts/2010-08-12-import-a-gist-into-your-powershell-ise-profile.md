---
layout: post
permalink: http://blog.stangroome.com/2010/08/12/import-a-gist-into-your-powershell-ise-profile/
title: Import a Gist into your PowerShell ISE profile
description: None
date: 2010-08-11 15:12:38 -0000
last_modified_at: 2010-08-11 15:17:57 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
I realised a few hours after posting [my last entry](http://blog.codeassassin.com/2010/08/11/automatic-tfs-check-out-for-powershell-ise/) about a PowerShell ISE profile script, that I failed to describe to people new to the ISE environment, how to include the script as part of their ISE profile so it is always available. Simply explaining how to determine the path of your ISE profile by checking the value of the $Profile variable from within the ISE would be boring, so instead I wrote another script to automatically download the first script from [GitHub](https://github.com/), and add it to your profile. This [new script](http://gist.github.com/519103) can simply be copied and pasted into the ISE's Command Pane (Ctrl+D) and your profile will be opened and updated, ready to be saved. Once saved, you can either restart the ISE or simply run " . $profile " (without quotes) from the Command Pane. You will hopefully notice in this new script, that my usual attention to neatness and verbosity has been replaced with terseness due to the purpose of this script being a use once and throw away item. I had considered writing it as a single line but quickly regained my senses.
