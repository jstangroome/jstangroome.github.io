---
layout: post
permalink: http://blog.stangroome.com/?p=119
title: The Horse That Will Not Die
description: None
date: 2007-04-21 22:03:34 -0000
last_modified_at: 2007-04-21 22:03:34 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

I am a VB.NET developer, I have been for a long time thanks to my background with old VB3 and BASIC before that. However, I like C# and I use it whenever an opportunity arises but due to business requirements the primary language with both my last employer and my current employer is VB.NET. It shouldn't matter much though because, due to IL, the Framework library and the Common Language Runtime, both VB.NET and C# compile down to very similar results.

Sadly, the VB.NET developers experience differs from the C# developers experience even though they now share the same IDE. It seems more and more regularly that I find myself making the comment to my colleagues at work that some particular issue would be easier to deal with if we were using C#. Here are some that come to mind:

C# has the ternary operator (<condition> ? <trueresult> : <falseresult>). This is a minor difference but can simplify some otherwise verbose code. VB.NET has the IIF function but it always returns Object instead of the type of the result, thereby requiring a cast, and it is not short-circuited, making it less effective at avoiding NullReferenceExceptions. Apparently, in Orcas, the IIF function gets the same behaviour as C#'s ternary operator.

C# has automated refactoring included. Thankfully, Microsoft arranged for DevExpress to provide a free version of Refactor! for VB.NET so we don't miss out completely. Unfortunately, it doesn't completely bridge the gap. A notable omission is the "Generate Method Stub" refactor that C# includes.

At work, we are slowly adopting Unit Testing where we can to help detect issues sooner in the development cycle. Exposing your production code for unit testing also helps to refactor it for better maintainability. Sometimes you can't easily test internal code but luckily the framework includes the  InternalsVisibleToAttribute which can allow a separate (unit testing) project to call Friend/Internal methods in another project. You can apply this attribute to any VB or C# code but only the C# compiler looks for it meaning we would need to write our unit tests in C#. Again, apparently Orcas has updated the VB compiler to fully support this attribute to.

As well as adopting Unit Testing, we are trying to establish a Continuous Integration system to discover the full impact of changes as they are checked-in. Unfortunately, while the VB IDE is smart enough to infer references beyond the first level, the MsBuild tool used by most CI systems does not, and therefore all builds fail. We could manually add the second level references to each of our projects but any well-meaning developer who uses the Remove Unused References button will undo the hard work. C# requires the second level references to be specified for even the IDE build so it doesn't have this issue. Apparently Orcas changes the VB IDE behaviour to match C# and MsBuild.

VB doesn't completely lose out though. VB has background compilation which makes on-the-fly error detection and Intellisense much more productive. It can slow things down in large solutions and it tends to be the cause of most of the IDE's crashes but in C#, where there is no background compiling, it is sorely missed.

In Visual Studio 2003, VB was behind C# in other ways. The VB team at Microsoft ensured VB received all the missing bits in Visual Studio 2005, however the C# team bounded forward with even more features for C#, thereby still leaving VB behind. With Orcas in Beta 1, we can see that VB has caught up again with the features C# already had. And once again, C# in Orcas has even newer features.

No Sir, I don't like it.

]]>
