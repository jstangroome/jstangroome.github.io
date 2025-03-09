---
layout: post
permalink: http://blog.stangroome.com/?p=120
title: Where's The Kaboom?
description: None
date: 2007-04-12 22:20:16 -0000
last_modified_at: 2007-04-12 22:20:16 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

My new home office machine has arrived and I am setting myself up for the most pain possible with my chosen configuration. Firstly I am installing Visual Studio 2005 on Vista Business Edition. Microsoft has only just released a patch for Visual Studio on Vista and it [isn't perfect](http://msdn2.microsoft.com/en-us/vstudio/aa964140.aspx "Visual Studio 2005 on Windows Vista Issue List"). Add to that running as a restricted user in Vista which introduces [more problems](http://msdn2.microsoft.com/en-us/vstudio/aa972193.aspx "Visual Studio 2005 on Windows Vista Issue List - Non Admin"). Finally I've chosen the 64-bit edition of Vista which complicates [working with Visual Studio](http://msdn2.microsoft.com/en-us/vstudio/aa964140.aspx#question43 "Code coverage binaries are not marked as 32-bit only causing them to crash on wow64") and [software in general](http://support.microsoft.com/kb/282423 "List of limitations in 64-Bit Windows").

Reading the many articles and forum posts about problems with Vista and 64-bit will discourage most people from trying but so far I've found the experience to be quite pleasant. The first road-block is the need for new Vista 64-bit signed hardware drivers but Windows detected everything but the onboard sound and I was able to very easily find drivers on the HP web site for that. Two vital utilities, [Daemon Tools](http://www.daemon-tools.cc/) and [UltraMon](http://www.realtimesoft.com/ultramon/), are already available with Vista x64 versions.

Vista has streamlined .zip file handling so I don't need any archiving software and the usual problems with new OS burning software are gone because Vista writes to CD and DVD out of the box. The small problem of burning disc images is solved by the free [ISO Recorder](http://isorecorder.alexfeinman.com/), updated for Vista x64. I don't need [MakeMeAdmin](http://blogs.msdn.com/aaron_margosis/archive/2004/07/24/193721.aspx) anymore because Vista's UAC temporary privilege elevation features have solved the problem. And, of course, Microsoft Office works beautifully.

I could rant for hours about the wonders of this new system but there were some issues. I managed to crash Visual Studio while setting the options for the first time but this is an easily avoidable [documented issue](http://msdn2.microsoft.com/en-us/vstudio/aa964140.aspx#question20 "Opening the Add-in/Macros Security option in the Tools dialog will hang Visual Studio") (thanks [Jim](http://www.nervoustych.com/)). I upgraded SQL Server 2005 to SP2 via Vista's built in AutoUpdate tool so I needed to run the [User Provisioning Tool](http://download.microsoft.com/download/2/B/5/2B5E5D37-9B17-423D-BC8F-B11ECD4195B4/ReadmeSQL2005SP2.htm#_windows_vista "Administrator Rights Not Inherited from Windows") manually afterward to be able to connect. Also, a minor annoyance, Windows x64 has a separate Program Files folder for 32-bit and 64-bit applications and some installers were defaulting to the wrong folder.

I'm keeping a list of issues I encounter with Windows in general and developing with Visual Studio and SQL Server. I'll write about my experiences further on this blog as time passes and as I start pushing the boundaries of compatibility. At this point I have no regrets moving to Vista and 64-bit. I sure don't want to go back to either 32-bit or Windows XP and I'd recommend anyone with similar needs to my own to do the same.

]]>
