---
layout: post
permalink: http://blog.stangroome.com/2012/02/23/requirements-for-a-powershell-module-manager/
title: Requirements for a PowerShell module manager
description: None
date: 2012-02-22 23:39:47 -0000
last_modified_at: 2012-03-22 11:27:55 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
I write a lot of PowerShell scripts. Many of these get transformed into modules for reuse and occasionally published for others. However making these reusable modules available wherever my scripts may run and propagating updates to these modules is not easily managed. As a solution to this I've been designing a PowerShell module to enable scripts and modules to easily reference and use other modules without requiring these modules to be installed first. The idea is very similar to [RubyGems](http://rubygems.org/) or [Nuget](http://nuget.org/) but I feel there are some important criteria that need to be met by a PowerShell module manager. Here is a list of requirements from the perspective of a module-consumer:
    * PowerShell already defines [standard install locations](http://msdn.microsoft.com/en-us/library/windows/desktop/dd878350\(v=vs.85\).aspx) for modules so a module manager should make it easy to use these by default but still allow them to be overridden.
    * Scripts will often run in a least privilege environment so a module manager should not require any special permissions to install a module.
    * PowerShell's Execution Policy depends on scripts that were downloaded from the Internet to be marked as such, a module manager should honour this by marking installed modules with the appropriate Zone Identifier.
    * PowerShell allows multiple versions of a module to exist side-by-side and allows scripts to reference a specific version, a module manager must support this behaviour.
    * A script should be able to use the module manager to install other modules even when the module manager itself hasn't been installed yet (eg using a secure bootstrap)
    * A script should be able to reference a module by name and optional version number only.
    * The module manager should operate on a clean operating system with only PowerShell installed and not have any other pre-requisites.
As a module author, there is also a set of expectations from a module manager:
    * A format for modules and their metadata is already [defined by PowerShell itself](http://msdn.microsoft.com/en-us/library/windows/desktop/dd878324\(v=vs.85\).aspx) (a PSD1 file with a collection of PSM1, PS1, and DLL files etc in a folder) so a module manager should leverage this instead of defining a new format. The format for transporting a module over the wire is an implementation concern of the module manager and not a concern of the module author.
    * Publishing a new or updated module for others to consume should be a PowerShell one-line command that can consume any locally installed module.
    * There should be a standard public repository for published modules but private repositories should also be supported for publishing modules intended for personal or enterprise use only.
There will inevitably be some impacts on how authors develop their modules to support the above requirements:
    * To support the common PowerShell Execution Policy setting of RemoteSigned, published modules should be signed with a valid Code Signing Certificate issued by a recognised authority. [As cheap as US$75](https://author.tucows.com/).
    * Modules must assume they will be executing in a least privilege environment and only operate with the minimum permissions required to perform the task the module is designed to do.
    * Related to least privilege, modules should not require a "first-use" process to register or configure the module that would require elevation.
    * Modules must not assume they will be installed to a specific location and instead should use the $PSScriptRoot variable for module-relative paths.
There are already some existing solutions for PowerShell module management, some meet most of the requirements above:
    * [Joel Bennett](https://twitter.com/#!/jaykul)'s [PoshCode module](http://poshcode.org/PoshCode.psm1) \- Been around the longest but currently only supports single-file modules (ie PSM1 files)
    * [Andrew Nurse](https://twitter.com/#!/anurse)'s [PS**-** Get](http://psget.org/) \- Almost a direct port of Nuget to PowerShell but requires PowerShell v2 to be configured to run under .NET 4.
    * [Mike Chaliy](https://twitter.com/#!/chaliy)'s [PSGet](http://psget.net/)\- Probably the closest to my ideal but currently it requires a GitHub pull-request to list a module and the module author must also find somewhere to host their module.
I think adding my own solution to this list would only add to the module management problem, especially when you consider the other PowerShell module management solutions that exist but I'm not aware of. Instead it would make more sense to have these existing solutions converge by having them each support the installation of modules published by the others and move toward meeting as many of the requirements above as feasible.
