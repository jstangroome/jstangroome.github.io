---
layout: post
permalink: http://blog.stangroome.com/2013/12/05/visual-studio-solutions-projects-and-shared-code/
title: Visual Studio Solutions, Projects, and shared code
description: None
date: 2013-12-05 11:05:58 -0000
last_modified_at: 2013-12-05 11:05:58 -0000
publish: true
pin: false
categories:
- Dot Net
- Visual Studio
tags: []
---
I have been having numerous discussions with a variety of people about shared code in .NET code bases and I decided to blog my thoughts on the topic here - partly to reduce repetition, partly to help me distill the concepts in my own mind. To clarify, these are my guidelines or rules of thumb. It is where I start when investigating options to improve handling shared code but I will bend these rules when required and I reserve the right to change my mind based on my future experiences. To begin, there seem to be two basic perspectives on the purpose of a Visual Studio Solution.
    1. A Solution is a container, a boundary. It includes everything required for a software system to be built and tested. The only dependencies external to the Solution are third party dependencies or internal dependencies from a package management system like NuGet.
    2. A Solution is a view, a window. It includes only the necessary items to work with a particular aspect of a software system. Projects within a Solution will routinely be dependent on other Projects not in the Solution. There will often be multiple Solutions that overlap with, or completely encompass, other Solutions.
I subscribe to the first group. I believe this is the model that Visual Studio guides developers toward through its default behaviours and through the challenges that arise when veering away from this model. I believe that a new team member should be able to clone a clean working set of source from version control and build the Solution and have all they need within the IDE. I like that a successful build of the open Solution (mostly) indicates that I haven't accidentally changed or removed code used elsewhere. To follow, given a common scenario of two mostly discrete Solutions that currently share a common Project between them, I start asking:
    * Can the Project be moved into a new, third Solution and packaged as a NuGet package? The original Solutions then reference this shared Project by its Package from (private) NuGet Repository. This can lengthen the feedback cycle when debugging, so if this leads to a poor experience because the shared Project is a common source of issues, a better suite of Integration Tests in the third Solution may help. If the shared Project changes often to implement features rather than fix bugs this may not be a good option.
    * Can the two Solutions be combined into one all-inclusive Solution? Would the new Solution then have too many Projects resulting a the build and/or test experience too slow or resource intensive? If the Project count is too high and code has been separated into Projects simply to enforce layer separation, perhaps some Projects can be consolidated and a tool like [NDepend](http://www.ndepend.com/) used to enforce separation.
    * Do the two Solutions together represent too large a system? Is the coupling to the shared Project an indication of a design that would benefit from significant refactoring - for example, favouring composition over inheritance.
Finally, what is the value of sharing the common Project? In my experience, increased code reuse is associated with higher coupling. Duplication of the shared code instead may prove beneficial in other stages of the delivery cycle and reduce each Solutions influence/impact on the other. I am also reminded of [Paul Stovell's short series of useful articles about Integration](http://paulstovell.com/blog/integration/scenario). The [Shared Database](http://paulstovell.com/blog/integration/shared-database) solution is an example where a Data Access Layer Project might be shared between two Solutions but the [Messaging](http://paulstovell.com/blog/integration/messaging) approach is an example where the two Solutions could be much more independent.
