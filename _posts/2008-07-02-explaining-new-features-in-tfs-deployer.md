---
layout: post
permalink: http://blog.stangroome.com/2008/07/02/explaining-new-features-in-tfs-deployer/
title: Explaining new features in TFS Deployer
description: None
date: 2008-07-02 13:35:22 -0000
last_modified_at: 2010-07-18 08:36:53 -0000
publish: true
pin: false
categories:
- TFS Deployer
tags: []
---
Since I began helping [Mitch](http://notgartner.wordpress.com/) with changes to [TFS Deployer](http://www.codeplex.com/tfsdeployer) to make it compatible with TFS 2008, I have slowly been adding small features that seemed useful in my work environment. However, unless you've read the code, you probably wouldn't know they exist so I'll describe them here. **Quality Mapping Wildcard** TFS Deployer operates on executing scripts for particular build quality transitions. Normally transitions like "Test" to "Production" are common. However you may wish to have a script that runs when the quality is set to "QA" regardless of what is was before. You can now specify asterisk (*) in place of the value of either of the OriginalQuality or the NewQuality attributes in the DeploymentMappings.xml file to specify a match on any quality. **Retain Build** Team Build in TFS 2008 now has Retention Policies to clean out old builds. If you deploy a build with TFS Deployer you don't want the policy deleting the build outputs. You may also want builds to become subject to policy again after you retire them from failed acceptance testing. You can add a RetainBuild attribute to a Mapping element in the D*M*.xml file. A value of "true" will mark the build to be kept upon successful execution of the deployment script. A value of "false" will unmark it, and an absent attribute will leave the retain flag alone. **Shared Deployment Resources** With multiple Team Builds defined and multiple deployment configurations you're bound to end up with scripts with common components you'd like to refactor out to a shared location. You can now specify a TFS path (ie $/MyProj/TeamBuildTypes/Shared/Deployment/) in the TfsDeployer.exe.config file in the SharedResourceServerPath application setting. If provided TFS Deployer will get the files in this directory and place them in the same folder as the deployment script on each script run. The first two features are in the [latest release drop](http://www.codeplex.com/tfsdeployer/Release/ProjectReleases.aspx) on CodePlex. The latter is only in the latest change set, so check the [Source Code](http://www.codeplex.com/tfsdeployer/SourceControl/ListDownloadableCommits.aspx) tab if you intend to use shared resources. Any problems, questions, or suggestions... let me know.
