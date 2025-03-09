---
layout: post
permalink: http://blog.stangroome.com/?p=35
title: Lambda for Regex
description: None
date: 2007-12-21 14:22:34 -0000
last_modified_at: 2010-10-06 12:05:13 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[ A few weeks ago I was trying to put together a quick regular expression for converting an entire English sentence into Pascal (or Headed Camel) Case. What would have been rather trivial in Perl was a little ugly in C#. Tonight, in a typical midnight haze, I realised I could use a lambda expression to tidy it up a bit. Here is the result, short and simple, presented in unit test form: [TestMethod] public void ShouldRemoveSpacesAndCapitaliseWords() {  const string original = "Should remove spaces and capitalise words";  const string expected = "ShouldRemoveSpacesAndCapitaliseWords";  Regex rx = new Regex(@"(\s*)\b(\w)");  string result = rx.Replace(original, m => m.Groups[2].Value.ToUpper()); Assert.AreEqual(expected, result); } ]]>
