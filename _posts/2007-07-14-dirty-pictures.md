---
layout: post
permalink: http://blog.stangroome.com/?p=73
title: Dirty Pictures
description: None
date: 2007-07-13 15:06:38 -0000
last_modified_at: 2010-07-18 23:23:00 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[ ![Example roadworks map](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/DirtyPictures_90/roadworkmap_1.png) Frustrated by the lack of up-to-date data for my [TomTom](http://www.codeassassin.com/blog/PermaLink,guid,6119fac2-2e5d-4764-b99c-1253360c66c7.aspx), I started poking around the web for various sorts of local information. I discovered that the [Department for Transport](http://www.transport.sa.gov.au/) has a [page listing current roadwork sites](http://www.transport.sa.gov.au/quicklinks/metro_country_roadworks.asp). Unfortunately, it is only available in HTML form. Upon inspecting the source however, I discovered patterns in the data suggesting it is automatically driven from by internal database. I asked myself, what is the quickest, easiest, most hackish way I can suck this information into a map? The answer was [Google Mapplets](http://www.google.com/apis/maps/documentation/mapplets/). The result, is an XML [Google Gadget](http://www.google.com/apis/gadgets/) that is hosted on my website but runs in the context of the [Google Maps](http://maps.google.com/) site and [screen scrapes](http://en.wikipedia.org/wiki/Screen_scraping) the DTEI's HTML using JavaScript. [Worse Than Failure](http://www.worsethanfailure.com/) would be proud. If you'd like to try it yourself, the [gadget xml file is here](http://www.codeassassin.com/exp/rwm.xml). You should be able to open Google Maps, click the My Maps tab and choose Add Content where you will be taken to the directory. Choose the Add By URL and paste the link to my xml file. I might tidy up the abomination that is my JavaScript code if anyone cares enough and submit the gadget to the public Google directory. I am also considering implementing the gadget in [Popfly](http://www.popfly.com/) but I am waiting on being able to sign-in to that site. ]]>
