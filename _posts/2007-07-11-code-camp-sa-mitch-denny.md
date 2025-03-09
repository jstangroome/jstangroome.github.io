---
layout: post
permalink: http://blog.stangroome.com/2007/07/11/code-camp-sa-mitch-denny/
title: 'Code Camp SA: Mitch Denny'
description: None
date: 2007-07-11 13:16:14 -0000
last_modified_at: 2010-07-18 08:41:41 -0000
publish: true
pin: false
categories:
- TFS
- TFS Deployer
tags: []
---
[Mitch](http://notgartner.wordpress.com/)'s presentation about "TFS Virtualization and Advanced Build Techniques" was my highlight for day one of Code Camp SA. Mitch suggested that one of the major benefits of virtualizing Team Foundation Server was the ability to rollback the whole server at any point. While that is certainly helpful if the service packs don't install right the first time, the benefit I've found personally is being able to pick up the whole VHD and drop it on another Virtual Server machine without any reconfiguration. Great for hardware failures. Mitch also suggested getting a *big* server to host the virtual TFS and use the slack space to host additional virtual build servers. We toyed with a build server once and based on performance I figured a physical machine would be best but Mitch demonstrated a great idea combining multiple virtual build servers with a false TFS build agent to auto-redirect to a free, pre-cleaned build server. In fact, the [Readify](http://www.readify.com.au/) team have some great TFS tools available to get the most from the system:

* [TFS Integrator](http://notgartner.wordpress.com/tag/projects/tfs-integrator/)
* [TFS Deployer](http://notgartner.wordpress.com/tag/projects/tfs-deployer/)
* [TFS Bug Snapper](http://www.holliday.com.au/display/ShowJournal?moduleId=349905&categoryId=47050)
* [Scenario Coverage Analyser](http://www.paulstovell.net/blog/index.php/scenario-coverage-analyser-for-tfs/)

Mitch showed how, with these tools and some PowerShell glue, you can have a complete, automated process to go from a check-in, automatic build, to update dependencies, rebuild those, deploy for QA, then deploy to production. Nice. With a lot of VB in our production code we've been held back from a CI system by the [MSBuild secondary reference issue](http://sstjean.blogspot.com/2006/11/msbuild-cant-find-secondary-references.html) but a quick chat with Mitch after the presentation cleared that up:

> Me: _"Doctor, it hurts when I do this."_ Mitch: _"Well, don't do that."_
