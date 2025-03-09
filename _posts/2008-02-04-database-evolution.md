---
layout: post
permalink: http://blog.stangroome.com/?p=33
title: Database Evolution
description: None
date: 2008-02-04 10:20:45 -0000
last_modified_at: 2008-02-04 10:20:45 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
K Scott Allen just recently posted [the final article in a series of five](http://odetocode.com/Blogs/scott/archive/2008/02/03/11746.aspx) about managing the development and deployment of a relational database alongside your code in a team environment.

He highlights some common failing points and good solutions to tricky obstacles, most of which I've faced throughout my career and learned about the hard way. I haven't quite achieved the same level of streamlined schema management that Scott has but it's good to know I'm on the right track.

[Visual Studio for Database Professionals](http://msdn2.microsoft.com/en-us/teamsystem/aa718807.aspx) projects have started to be included in our product source control and we are pushing this great but still relatively new tool to automate as much of our schema management as possible. No matter how good the tools are though, I'm not [the only one](http://odetocode.com/Blogs/scott/archive/2008/02/02/11721.aspx#11734) who feels at least a few developers on any team "need to step up and get comfortable with SQL".

In fact, getting comfortable with effective use of PowerShell, in addition to SQL, has helped to get reams of lookup data (ie post codes, etc) into repeatable T-SQL scripts and also to manage deployment of schema changes to multiple sites.

I've also just ordered [Refactoring Databases: Evolutionary Database Design](http://www.amazon.com/Refactoring-Databases-Evolutionary-Addison-Wesley-Signature/dp/0321293533) by [Scott Ambler](http://www.ambysoft.com/) and [Pramod Sadalage](http://www.sadalage.com/). The book focuses on applying the same incremental refactoring techniques used in code to evolve a database schema over time with minimal upset to existing systems. I expect to find some real insight into database development in this book.

Ambler has recently recorded some good interviews on both [.NET Rocks](http://www.dotnetrocks.com/default.aspx?showNum=210) and [OnSoftware](http://www.informit.com/podcasts/episode.aspx?e=e316422d-b974-4125-98d0-f0e9c5c18a5b) and Pramod has an e-book on [Recipes For Continuous Database Integration](http://www.informit.com/store/product.aspx?isbn=032150206X) worth investigating.
