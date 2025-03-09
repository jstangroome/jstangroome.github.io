---
layout: post
permalink: http://blog.stangroome.com/?p=114
title: The Diet Coke Of Unit Testing?
description: None
date: 2007-05-19 07:29:36 -0000
last_modified_at: 2007-05-19 07:29:36 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

I have been wanting to make use of Unit Testing in my personal projects and my projects at work. So far I've read a lot about unit testing concepts and implementations and even written a few tests to get a feel for it. The majority of the material that exists about unit testing in .NET refers to the [NUnit](http://sourceforge.net/projects/nunit) testing framework. The is presumably because it was one of the first .NET unit testing frameworks and based on the well established JUnit. However, now that Visual Studio 2005 includes it's own unit testing framework, why wouldn't you want to use that?

[Jean-Paul Boodhoo](http://www.jpboodhoo.com/blog/) [once blogged](http://www.jpboodhoo.com/blog/AutomatingYourBuildsWithNAntPart1.aspx)1 that when a new developer joins your team they should be able to Get Latest from source control, build, and be ready to work. I agree and using tools included in Visual Studio means one less dependency for a new developer to install on their workstation. However, there is the possibility that Visual Studio's Unit Testing, as a version 1.0 product, might be lacking important functionality to be found in established testing tools. A quick Google for "[visual studio unit testing vs nunit](http://www.google.com/search?q=visual+studio+unit+testing+vs+nunit)" leads to a few notable issues.

[Jason Anderson explains](http://blogs.msdn.com/jason_anderson/archive/2004/06/04/148252.aspx) that only Visual Studio Team Suite includes the Unit Testing tools and that Visual Studio Professional misses out. We all use Team Suite at work so this isn't a problem but I can understand that unit testing should be available developers in all team sizes. Thankfully, Naysawn Naderi says that we can expect [Unit Testing in Visual Studio Pro](http://blogs.msdn.com/nnaderi/archive/2007/03/27/unit-testing-trickling-into-pro.aspx) when Orcas is released.

Roy Osherove also has [two](http://weblogs.asp.net/rosherove/archive/2006/01/25/436414.aspx "Bugs and Missing Features with Team System Unit Testing: Part I") [posts](http://weblogs.asp.net/rosherove/archive/2006/09/24/MbUnit-vs.-NUnit-Vs.-Team-System-Unit-Testing-_2D00_-Choosing-a-unit-test-framework.aspx "MbUnit vs. NUnit Vs. Team System Unit Testing - Choosing a unit test framework") about choosing between Visual Studio Unit Testing and the alternatives. Roy's comments makes VS Unit Testing seem completely useless and claims some parts are broken and even says "how this one went by into the release is a mystery to me". However, I recently had the pleasure of working with VS Unit Testing briefly when contributing to the [BlogML](http://www.codeplex.com/BlogML) project and some of Roy's claims just felt wrong. Surely it can't be that bad. Considering we will want to decide on a testing framework for projects at work soon, I decided to verify these problems.

Roy's first complaint is that there is a bug with the ExpectedException Attribute checking the type of the Exception only and not checking the Message. This is in fact a misinterpretation of the VS Unit Testing ExpectedException Attribute and assuming it was designed the same as NUnit's counterpart. NUnit takes an Exception type and message and verifies that the right Exception is thrown and that is has the matching message. VS treats the message parameter that same as an Assert statement and uses it as the display message to the user when the test fails, admittedly the MSDN documentation is very poorly worded. However, achieving the same behaviour as NUnit is often desired and there is a quick solution:

[TestMethod()]  
[ExpectedException(typeof(InvalidOperationException))]  
public void ExceptionWithMessage()  
{  
    try  
    {  
        MethodToTest();  
    }  
    catch (InvalidOperationException ex)  
    {  
        Assert.AreEqual("Expected message.", ex.Message);  
        throw;  
    }  
}

Roy is also disappointed that ExpectedException doesn't work with the base Exception type. Sure it's something the unit testing framework doesn't support but you shouldn't be throwing Exception objects and [you shouldn't be catching them](http://blogs.msdn.com/fxcop/archive/2006/06/14/631923.aspx) either. You can also use the Test Results window to group by Result (ie Passed/Failed), Class Name, and Full Class Name (ie Namespace too), this makes locating the tests your interested in much easier and also negates Roy's corresponding observations with regard to Test Driven Development.

VS Unit Testing isn't perfect though and that's to be expected for a first release. As Roy points out, you can't easily get to the failing production code from a failed test result, it doesn't have quite as many included Asserts as NUnit does and you do need to have Visual Studio installed to use the MSTest command line test runner. For the time being, our requirements don't push VS beyond it's capabilities, Orcas should improve on the weaker points when it's released later this year ([?](http://weblogs.asp.net/fmarguerie/archive/2006/05/17/first-orcas-release-date-estimate.aspx)) and it is really easy to [convert between NUnit and VS Unit Testing](http://www.google.com/search?q=convert+nunit) if we need to.

So, if you're looking to choose a unit testing framework, don't be put off by some of the articles flying around about VS. Give NUnit and VS Unit Testing a go and maybe [MbUnit](http://www.mbunit.com/) and some of the [others](http://en.wikipedia.org/wiki/List_of_unit_testing_frameworks#.NET_programming_languages) too. Ultimately you'll find something that you and the rest of your team are comfortable with and fits your process. I can't say that any of them are perfect, nor are any flawed beyond useful. And, if you've managed to read this entire post, thanks for putting up with my ramblings ;).

* * *

1\. Update: Added link to Jean-Paul Boodhoo's article.

[![kick it on DotNetKicks.com](http://www.dotnetkicks.com/Services/Images/KickItImageGenerator.ashx?url=http://www.codeassassin.com/blog/PermaLink,guid,a05d52ba-ef83-4c33-a735-56a7fc55eca8.aspx)](http://www.dotnetkicks.com/kick/?url=http://www.codeassassin.com/blog/PermaLink,guid,a05d52ba-ef83-4c33-a735-56a7fc55eca8.aspx)

]]>
