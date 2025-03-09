---
layout: post
permalink: http://blog.stangroome.com/?p=109
title: Finding Closure
description: None
date: 2007-06-04 21:19:57 -0000
last_modified_at: 2007-06-04 21:19:57 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
[Scott Hanselman](http://www.hanselman.com/blog/) recently [posted about troubles with external hard drives](http://www.hanselman.com/blog/TheDuhFilesTheFileIsTooLargeForTheDestinationFileSystem.aspx) and large files. However, while file systems and compatibility between multiple OSes is quite a problem on it's own (and one I'll still need to [solve for myself](http://www.lacie.com/au/products/product.htm?pid=10844) soon), I've been wrestling with a different large file issue.

Several months ago I purchased (separately) a Samsung 40GB 2.5" PATA hard drive and a USB external hard drive enclosure to suit for quite a low price from eBay. Most of my data trafficking between home and work consisted of Office documents, source code, and other assorted small files. All was well.

Since then, I have needed to transfer Virtual PC images, MSDN DVD images, and server backups and I have had no success. Without any error messages during the copy process I was finding that files over ~400MB copied to the external drive were corrupt. They had the same length as the original but a command line [FC /B](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/fc.mspx) or an [FCIV](http://support.microsoft.com/kb/841290) revealed that random bytes were different.

Extensive Googling suggested motherboard USB power issues, cable quality problems, hard drive faults, and write-caching settings as potential causes. I methodically checked each possibility by using different machines, PCI USB controllers, new cables, a different 2.5" hard drive, and playing around in Device Manager. Nothing helped.

A few weeks ago, my local computer store had a special on [Western Digital Passport Portable Drives](http://www.westerndigital.com/en/products/products.asp?driveid=262). The Western Digital brand has always been good to me and I figured I could try one and have someone to complain to if it didn't work. I bought one and since then I've been moving all sorts of large files around (including 16GB Windows backups) without any problems.

That's just one more time for me, learning the hard way to stop buying cheap components and instead to just fork out for the quality brands up front. It's [like washing a spoon under a running tap](http://secretgeek.net/onmetaphors.asp): occasionally you forget what happened last time and you get wet. So if you're getting corrupt files on an hard drive in an external enclosure, just cough up the cash and buy something better.
