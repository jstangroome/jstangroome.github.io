---
layout: post
permalink: http://blog.stangroome.com/?p=29
title: The Next Step For VS2008 Database Edition
description: None
date: 2008-04-11 13:49:51 -0000
last_modified_at: 2008-04-11 13:49:51 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I started using [VSTS Database Edition](http://msdn2.microsoft.com/en-au/vsts2008/products/bb933747.aspx) back when it was called Data Dude and available as a CTP download. Since then I have slowly embraced all its features and I now use it as my complete database development solution from schema management, to data generation, and finally deployment.

DB Edition has its quirks but you learn to understand them and work with them and each new version has new features to improve the workflow. However, as I have been using DB Edition each day, an idea has been steadily stewing in my head for where I'd like to see DB Edition go next. My thought process in a nut-shell follows...

A Database Project allows you to define your schema and data generation and, from a Visual Studio context menu, deploy the database to a chosen server as a new database or as an upgrade to an existing database. You can also use the [DatabaseTestService](http://msdn2.microsoft.com/en-us/library/microsoft.visualstudio.teamsystem.data.unittesting.databasetestservice.aspx) in a test project to deploy the database and test data for automated testing. And finally you can use the SqlDeploy MSBuild task to deploy the database as part of a continuous integration build.

However, all of these methods of deployment require the relative path to the Database Project and all its SQL scripts and some settings in a configuration file. This causes problems in deployed test runs and Team Build test runs where the relative path to Database Project often changes. It also restricts using multiple test databases in a single test project due to the way the configuration file works.

I propose that, at build time, DB Edition could package into a .NET assembly, the full definition of the Database Project along with some standard bootstrap code but minus the deployment configuration. Test projects could then include a reference to the DB assembly and make calls into the bootstrap code, perhaps something in the form:

MyDBAssembly.Schema.DeployTo(someConnectionString, someOptions);  
MyDBAssembly.SomeDataGenPlan.DeployTo(someConnectionString);

The DB assembly will be treated as a dependency like any other references would and will naturally be moved around wherever the primary assembly gets deployed and all the necessary information will always be available to perform a completely new database deployment or to perform a schema upgrade on any compatible existing database instance.

If the DB assembly happened to double as a console application, it could be used for ad hoc command-line deployments or even included in batch, MSBuild, or PowerShell scripts for automated deployments.

I am contemplating several ways to hack a feature like this into DB Edition myself but I'm hoping someone else has already done it or maybe the [DB Edition team](http://blogs.msdn.com/gertd/) already has it on the cards for Rosario.
