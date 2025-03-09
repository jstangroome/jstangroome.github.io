---
layout: post
permalink: http://blog.stangroome.com/2007/09/12/powershell-regular-expression-compiler/
title: PowerShell Regular Expression Compiler
description: None
date: 2007-09-11 14:40:23 -0000
last_modified_at: 2010-07-18 22:30:29 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
As an exercise in the mostly useless, I decided it would be interesting to try my hand at writing a PowerShell script to compile a regular expression pattern to a .NET assembly. I sure someone has already created a NAnt or MSBuild task to do this but I felt this would be a good way to increase my familiarity with PowerShell. The script requires the RegEx pattern and an output type name and full namespace to work. You can optionally pass an AssemblyName but if omitted the type and namespace will be used to form the output file name. You can also specify the -ignoreCase or -multiLine switches to enable that behaviour on your expression. There are other options that probably should be supported but they can be easily added if you actually have a use for this. Without further delay, here is the code: param ( [string] $pattern = "", [string] $typeName = "", [string] $fullNamespace = "", [System.Reflection.AssemblyName] $assemblyName = $null, [switch] $ignoreCase, [switch] $multiLine ) if ($pattern -eq "") { throw ("-pattern required"); } if ($typeName -eq "") { throw ("-typeName required"); } if ($fullNamespace -eq "") { throw ("-fullNamespace required"); } if ($assemblyName -eq $null) { $assemblyName = New-Object System.Reflection.AssemblyName ` ($fullNamespace + "." + $typeName); $assemblyName.Version = New-Object System.Version (1, 0); } $sysRx = @{}; $sysRx.Namespace = "System.Text.RegularExpressions"; $sysRx.RegEx = [System.Text.RegularExpressions.RegEx]; $sysRx.RegExOptions = [System.Text.RegularExpressions.RegExOptions]; $options = $sysRx.RegExOptions::None; if ($ignoreCase) { $options = $options -bor $sysRx.RegExOptions::IgnoreCase; } if ($multiLine) { $options = $options -bor $sysRx.RegExOptions::Multiline; } $info = New-Object ($sysRx.Namespace+".RegexCompilationInfo") ` ($pattern, $options, $typeName, $fullNamespace, $true); $popDir = [System.Environment]::CurrentDirectory; [System.Environment]::CurrentDirectory = $PWD; $sysRx.RegEx::CompileToAssembly($info, $assemblyName); [System.Environment]::CurrentDirectory = $popDir; Here is an example usage: ./Compile-RegEx.ps1 "\s+" "Spaces" "CodeAssassin.RegEx" -ignoreCase
