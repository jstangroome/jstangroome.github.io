---
layout: post
permalink: http://blog.stangroome.com/2010/08/03/create-scvmm-templates-with-powershell/
title: Create SCVMM Templates with PowerShell
description: None
date: 2010-08-03 12:32:06 -0000
last_modified_at: 2010-08-03 12:32:06 -0000
publish: true
pin: false
categories:
- PowerShell
- SCVMM
tags: []
---
Now that I get to work with a TFS 2010 Lab Management environment most days, I find myself building various virtual machines to replicate the production environments of our clients for testing. With many different clients and projects, the range of virtual machine operating systems expands exponentially as matrix of core OS version, processor architecture, service pack, IE version, and other minor variations. However, for any particular configuration, I'll also want multiple copies so naturally I want to make use of System Center Virtual Machine Manager's VM Template Library. However, creating a template from a VM using the SCVMM Administrator Console, without destroying the original VM, is death by a thousand clicks:

  1. Shutdown the VM.
  2. Dismount any media in the VM's virtual DVD drives.
  3. Clone the VM via an eight-page wizard.
  4. Wait for the cloning to complete.
  5. Convert the cloned VM into a template with a six-page wizard.
  6. Wait for the sysprepping to complete.
  7. Restart the original VM.

I tire of such tedium very quickly and as such, I've scripted the above process with PowerShell and the SCVMM Snapin. You can access the [Export-VMTemplate script](http://gist.github.com/506252) I've written on gist.github. If you open a PowerShell window from the toolbar in the SCVMM console, you can execute the script like this:
  
    .\Export-VMTemplate.ps1 -VM 'MyOriginalVm' -TemplateName 'NewTemplateName' -LibraryServer 'MyLibSvr'

Hopefully the script itself, or at least some of the concepts within, will be useful to someone else.
