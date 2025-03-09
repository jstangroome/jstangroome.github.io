---
layout: post
permalink: http://blog.stangroome.com/2013/08/02/powershell-update-help-and-an-authenticating-proxy/
title: PowerShell Update-Help and an Authenticating Proxy
description: None
date: 2013-08-01 22:24:41 -0000
last_modified_at: 2013-08-01 22:24:41 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
PowerShell v3 doesn't ship with help in the box anymore. You may love this or you may hate it. Regardless of your stance, if your environment is behind an authenticating web proxy, it is not obvious how to make it work. [The general guidance](http://blogs.technet.com/b/heyscriptingguy/archive/2012/08/31/understanding-and-using-updatable-powershell-help.aspx) is to use Save-Help from another computer but this doesn't help when every computer is behind the proxy and [sneakernet](http://en.wikipedia.org/wiki/Sneakernet) is prohibited. This was my situation recently and I found a reasonably simple way to solve it. Firstly, when trying to run Update-Help, I received an error message:
  
        update-help : Failed to update Help for the module(s) ' ... ' with UI culture(s) {en-US} :
    Unable to connect to Help content. Make sure the server is available and then try the command again.

Unfortunately, nothing about this message tells me that the problem is caused by the proxy, let alone authentication. Having dealt with the proxy in this environment before I had a suspicion, so I opened [Fiddler](http://fiddler2.com/) (an HTTP debugger) and re-attempted the Update-Help command. Fiddler revealed that the HTTP response returned to PowerShell was status code 407, "Proxy authentication required", so I was on the right track. Inspecting [Update-Help's available parameters](http://technet.microsoft.com/en-us/library/hh849720.aspx) reveals two that might be relevant: Credential and UseDefaultCredentials. To save you some time, I can tell you that using either of these won't help a proxy authentication issue. Under the hood, Update-Help is using [.NET's WebClient class](http://msdn.microsoft.com/en-us/library/system.net.webclient.aspx) to download the help information and these credential-related parameters correspond directly to properties of the same name on WebClient which don't apply to proxies. Separately though, WebClient has a Proxy property which in turn has a Credentials property - this is the one I needed to find a way to set. Luckily, even though the WebClient instance used within the implementation of the Update-Help cmdlet is not exposed, by default all instances of WebClient in the same AppDomain will share the same instance of their Proxy. So, firstly, to verify my theory, I run these commands:
  
        $wc = New-Object System.Net.WebClient
    $wc.DownloadString('http://microsoft.com')

And get the expected error:
  
        Exception calling "DownloadString" with "1" argument(s):
    "The remote server returned an error: (407) Proxy Authentication Required."

So I set the credentials on my test WebClient's Proxy to use my current credentials that I logged in to Windows with, and test again:
  
        $wc.Proxy.Credentials = [System.Net.CredentialCache]::DefaultNetworkCredentials
    $wc.DownloadString('http://microsoft.com')

And this time I get the raw content of the Microsoft.com home page instead of an error. I now try to run Update-Help one more time and bingo! - it works. Now we just need to encourage Microsoft to make this easier to deal with in general by voting for [these](http://connect.microsoft.com/PowerShell/feedback/details/730195/save-help-failure-unable-to-connect-to-help-content-error-while-connected-to-internet-via-corporate-proxy-server) [two](http://connect.microsoft.com/PowerShell/feedback/details/754102/a-cmdlet-to-create-a-proxy-configuration-settings-object) issues on Connect.
