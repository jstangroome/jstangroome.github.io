---
layout: post
permalink: http://blog.stangroome.com/2009/02/14/analyse-code-coverage-with-powershell/
title: Analyse Code Coverage with PowerShell
description: None
date: 2009-02-13 14:28:20 -0000
last_modified_at: 2010-07-18 09:56:17 -0000
publish: true
pin: false
categories:
- PowerShell
- Visual Studio
tags: []
---
Visual Studio 2008 Team System's built-in Code Coverage is nice but the standard results window only allows you to drill down through each assembly, then namespace, class, and finally method. You can't easily find the class with the least blocks covered, something I needed to do the other day. I found [John Cunningham's blog about "off-road" code coverage](http://blogs.msdn.com/ms_joc/articles/406608.aspx) and was pleased to see that Microsoft had provided an assembly in Visual Studio that can be used to parse the *.coverage file output by a test run. I followed his example to write a PowerShell script to provide basic access to the data. You can [download my script here](http://www.codeassassin.com/public/Get-CodeCoverageDataSet.zip). Then you can use it like this:
  
    $CoverageDS = ./Get-CodeCoverageDataSet.ps1 "data.coverage"
    $CoverageDS.Class `
      | Sort-Object -Property BlocksNotCovered -Descending `
      | Select-Object `
        -First 25 `
        -Property `
          BlocksNotCovered, `
          @{
            Name = "Namespace";
            Expression = {
              $CoverageDS.NamespaceTable.FindByNamespaceKeyName($_.NamespaceKeyName).NamespaceName
            }
          }, `
          ClassName

The coverage file is typically found in the TestResults\\[TestRunName]\In\\[ComputerName]\ folder. You can easily perform queries over methods or lines rather than classes by using the other tables in the returned dataset. You can also use the ConvertTo-Html cmdlet to easily create a report for your team.
