---
layout: post
permalink: http://blog.stangroome.com/?p=98
title: Premature Analysation
description: None
date: 2007-06-14 13:15:01 -0000
last_modified_at: 2007-06-14 13:15:01 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

I was thinking today as I was resolving a few [Code Analysis](http://msdn2.microsoft.com/en-us/library/bb429476\(VS.80\).aspx) violations that perhaps I should stop. I will assume you are familiar with the quote "[Premature optimisation is the root of all evil](http://www.google.com/search?q=premature+optimization+is+the+root+of+all+evil)" and if you're not, [you](http://en.wikipedia.org/wiki/C._A._R._Hoare) [should](http://en.wikipedia.org/wiki/Donald_Knuth) [be](http://www.acm.org/ubiquity/views/v7i24_fallacy.html).

I've often had the feeling that something wasn't quite right when I adjusted my code to satisfy the various CA [performance warnings](http://msdn2.microsoft.com/en-us/library/ms182260\(VS.80\).aspx). Today I have investigated that feeling further:

**[![Winner of Zurich Marathon 2007](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/PrematureAnalysation_1336E/marathonportrait_1.jpg)](http://www.flickr.com/photos/fotomaker/441778648/)CA1809** : [Avoid excessive locals](http://msdn2.microsoft.com/en-us/library/ms182263\(VS.80\).aspx) \- Triggers when you have more than 64 local variables in a method because the processor registers can no longer hold them all and some will be moved to and from the stack. If you have this many variables in a single method you have other problems to worry about.

**CA1811** : [Avoid uncalled private code](http://msdn2.microsoft.com/en-us/library/ms182264\(VS.80\).aspx) \- Triggers when a private method exists that isn't called directly. This is great for finding and removing dead code but this is more about readability and not runtime performance.

**CA1812** : [Avoid uninstantiated internal classes](http://msdn2.microsoft.com/en-us/library/ms182265\(VS.80\).aspx) \- Very similar to CA1811 but at the class level. Once again, good for removing dead code but not really performance related.

**CA1807** : [Avoid unnecessary string creation](http://msdn2.microsoft.com/en-us/library/ms182266\(VS.80\).aspx) \- This is for people who still insist of using ToUpper or ToLower on strings before performing comparisons. Embrace the Unicode world and pass the [StringComparison](http://msdn2.microsoft.com/en-us/library/system.stringcomparison.aspx) enumeration to String.Equals and String.Compare calls. This rule belongs in the Globalization set.

**CA1813** : [Avoid unsealed attributes](http://msdn2.microsoft.com/en-us/library/ms182267\(VS.80\).aspx) \- This might be the first real performance rule so far but it does apply specifically to reflection on custom attributes. If your application makes heavy use of attributes, you may want to pay attention here. I think though that your personal philosophy on preferring extendable classes versus sealed classes may have more weight here.

**CA1801** : [Review unused parameters](http://msdn2.microsoft.com/en-us/library/ms182268\(VS.80\).aspx) \- Once again, this is an excellent rule for locating dead code that may be misleading or unnecessarily complicated to maintain. I am beginning to wonder if the Microsoft.Performance CA category is a misnomer.

This covers the first third of the 18 rules on the [performance warnings list](http://msdn2.microsoft.com/en-us/library/ms182260\(VS.80\).aspx). So far only one could potentially undermine the performance of your application given the right conditions and I'd run an actual test to time this before I started making big changes.

However, the rules shouldn't be ignored, perhaps they should just be considered from a point of view other than performance. Hopefully investigating the rest of the list, which I plan to do in future posts, will reveal valid rules for improving performance. I'm expecting at least [CA1818](http://msdn2.microsoft.com/en-us/library/ms182272\(VS.80\).aspx) to come though for me there.

]]>
