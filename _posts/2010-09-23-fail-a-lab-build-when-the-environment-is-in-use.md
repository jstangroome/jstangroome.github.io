---
layout: post
permalink: http://blog.stangroome.com/2010/09/23/fail-a-lab-build-when-the-environment-is-in-use/
title: Fail a Lab build when the environment is in use
description: None
date: 2010-09-23 08:23:59 -0000
last_modified_at: 2010-09-23 08:23:59 -0000
publish: true
pin: false
categories:
- TFS
tags: []
---
![Screenshot of how to mark a lab environment as "in use".](http://blog.stangroome.com/wp-content/uploads/2010/09/markinuse.png)Microsoft Test Manager in Team Foundation Server 2010 enables users to mark a lab environment as "in use" with their name, time stamp, and a comment. Other people can then see this and contact the user first if they'd also like to test, redeploy or otherwise interrupt a lab environment. [![Screenshot of a lab environment marked as "In Use"](http://blog.stangroome.com/wp-content/uploads/2010/09/markedinuse.png)](http://blog.stangroome.com/wp-content/uploads/2010/09/markedinuse.png) However, if a user manually queues a Lab build from Visual Studio Team Explorer, or a Lab build is triggered by a schedule or a check-in, this marker is not visible and the Lab build will execute, redeploying the environment and most likely upsetting the user who was already using the environment. As a solution to this, I have modified the default lab build process template to include a new parameter to specify whether the build should fail if the environment has been marked as "in use". [![Screenshot of the added build process parameter](http://blog.stangroome.com/wp-content/uploads/2010/09/parameters.png)](http://blog.stangroome.com/wp-content/uploads/2010/09/parameters.png) Thankfully, most of the hard work has already been done in the form of the out-of-the-box Workflow activities for Lab Management, and I just needed to add a small chunk of XAML to the existing LabDefaultTemplate.xaml file in my Team Project's "BuildProcessTemplates" source control folder. The[full customised template is here](http://gist.github.com/593319), but the core of the change was this simple code: [![](http://blog.stangroome.com/wp-content/uploads/2010/09/code.png)](http://blog.stangroome.com/wp-content/uploads/2010/09/code.png) And now the lab build will fail when the environment is marked "in use"... [![](http://blog.stangroome.com/wp-content/uploads/2010/09/failed.png)](http://blog.stangroome.com/wp-content/uploads/2010/09/failed.png)
