---
layout: post
permalink: http://blog.stangroome.com/2013/08/13/refactor-test-methods-associated-with-test-manager-test-case-automation/
title: Refactor Test Methods associated with Test Manager Test Case Automation
description: None
date: 2013-08-13 09:55:19 -0000
last_modified_at: 2013-08-13 09:55:19 -0000
publish: true
pin: false
categories:
- Coded UI
- PowerShell
tags: []
---
Since Team Foundation Server 2010, Microsoft has shipped the Test Manager product which enables testers to document manual test cases and later automate them (or just automated them from the beginning). When used to its full potential with other TFS components like Team Build and Lab Management, managing Test Cases with TFS provides a great way to improve and verify the quality of the software you deliver. In Test Manager, or more accurately Visual Studio, automation for a Test Case is associated via two primary identifiers:
    1. The file name of the .NET Assembly containing the relevant Test Method, eg "CodedUITestProject1.dll"
    2. The namespace- and class-qualified name of the Test Method, eg "CodedUITestProject1.CodedUITest1.CodedUITestMethod1"
After associating one or more Test Methods with Test Cases you may later need to refactor your code resulting in many Test Methods moving to a new class or namespace, or even a new assembly name. Unfortunately, after this refactoring effort, Microsoft Test Manager will no longer be able to locate the Test Methods associated with affected Test Cases and the tests will fail to run. There are some options for fixing this, of varying effectiveness:
    * Undo the refactoring - pointless but technically an option.
    * Manually open each one of the Test Cases in Visual Studio and re-associate the automation - tedious for larger numbers of tests.
    * If only the assembly name has changed, open the list of Test Cases in Excel, include the "Automated Test Storage" column, and bulk update the values.
BUT, if the namespace or class has changed, the fix could be slightly more complicated... You may be able to use Excel with the "Automated Test Name" column included and you may find it works but there is also another column to consider, usually hidden, called "Automated Test Id". I know this column is used by the [TCM command-line tool to automatically import Test Methods into Test Cases](http://msdn.microsoft.com/en-us/library/ff942471.aspx) and it may be used in other areas of Visual Studio and TFS. In tcm.exe, the Id is based on the Test Name and used to detect if a Test Method has already been imported and to prevent the creation of duplicate Test Cases. [Gautam Goenka's blog post on associating automation programmatically](http://blogs.msdn.com/b/gautamg/archive/2012/01/01/how-to-associate-automation-programmatically.aspx) demonstrates the simple Id generation algorithm used. As per my usual style, I've published a PowerShell script, called ["Rename-TestCaseAutomation" on GitHub Gist](https://gist.github.com/6075788), which will locate all the Test Cases with associated automation using a specified Assembly Name and/or Class Name prefix and update them with a provided new Assembly Name or Class Name Prefix, and update the Id appropriately too. Here are two simple example scenarios for using it: Rename the Assembly for all Test Cases across two different Team Projects:
  
        Rename-TestCaseAutomation http://localhost:8080/tfs/DefaultCollection MyProject,YourProject -TestStorage CodedUITestProject1.dll -NewTestStorage FooTests.dll

Move all the Tests Methods in the "FooTests.Customer" Class to to the "FooTests.Client" Class:
  
        Rename-TestCaseAutomation http://localhost:8080/tfs/DefaultCollection MyProject -TestNamePrefix FooTests.Customer -NewTestNamePrefix FooTests.Client

Hopefully someone else finds this useful.
