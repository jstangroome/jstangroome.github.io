---
layout: post
permalink: http://blog.stangroome.com/2008/07/05/tfs-deployer-least-privilege/
title: TFS Deployer Least Privilege
description: None
date: 2008-07-05 00:54:16 -0000
last_modified_at: 2010-08-03 12:43:38 -0000
publish: true
pin: false
categories:
- TFS Deployer
tags: []
---
The original [TFS Deployer](http://www.codeplex.com/tfsdeployer) attempted to access various restricted resources than effectively required it to run as an administrator user. Since some changes were made in February, Deployer can now be executed under a standard user account with some additional considerations which follow. **Team Foundation Server Permissions** The TFS Deployer service account must be a member of the Team Foundation Administrators server-level group on the server specified by the TeamFoundationServerUrl config setting. The permission to subscribe to Build Quality changes via SOAP is only granted to adminstrators. I am investigating a way to grant this permission at a more granular level. **PowerShell Execution Policy** TFS Deployer can execute Windows Command Scripts if you really need it to but PowerShell scripts are the preferred option. However, PowerShell is built to be secure-by-default and installs with script-running capabilities disabled. It is left for the user to enable scripts after PowerShell has been installed and can be performed by opening the PowerShell console as an administrator and executing either:
  
    Set-ExecutionPolicy AllSigned ; # orSet-ExecutionPolicy RemoteSigned

The former choice will require you to sign all your deployment scripts in source control with a certificate trusted by the Deployer. [Scott Hanselman](http://www.hanselman.com/blog/) has a comprehensive post on his blog describing the [signing process](http://www.hanselman.com/blog/SigningPowerShellScripts.aspx). The latter choice [will not impose this requirement](http://blogs.msdn.com/powershell/archive/2007/03/07/how-does-the-remotesigned-execution-policy-work.aspx) because files retrieved from source control by Deployer are not marked with a zone identifier. **HTTP Namespace Reservation** TFS Deployer utilises self-hosted WCF over HTTP to subscribe to Build Quality Change events from TFS. This technique relies on the Windows HTTP Server API which requires the Deployer service account to have been granted rights to listen on the portion of the HTTP URL namespace it uses for event notification. By default only local administrators have access to the entire namespace and permissions must be granted using the httpcfg tool (on Windows XP and Server 2003) or the netsh tool (on Vista and Server 2008). Using the netsh tool, from an administrator command prompt, the following command (as a single line) will grant rights to the appropriate URL for TFS Deployer (assuming you are using the default port number):
  
    netsh http add urlacl url=http://+:8881/BuildStatusChangeEvent user=DOMAIN\TfsDeployerUser

[Using the httpcfg tool](http://msdn.microsoft.com/en-us/library/ms733768.aspx) is more difficult as it requires an ACL string in Security Descriptor Definition Language (SDDL). However, [Dominick Baier](http://www.leastprivilege.com/) has [created a tool](http://www.leastprivilege.com/URIACLsAndWCFRevisited.aspx) to simplify this process and I can recommend it. **Additional Permissions** All deployment scripts executed by TFS Deployer will be executed under the context of the service account unless the scripts explicitly contain alternate credentials. If the scripts copy files to a network share, configure a web site in IIS, or even upgrade the schema of a SQL database, ensure that the service account also has the minimum privileges to perform those actions too. Any problems, questions, or suggestions... let me know.
