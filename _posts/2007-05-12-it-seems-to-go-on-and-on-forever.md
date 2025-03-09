---
layout: post
permalink: http://blog.stangroome.com/?p=116
title: It Seems To Go On And On Forever
description: None
date: 2007-05-11 14:37:15 -0000
last_modified_at: 2007-05-11 14:37:15 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Those people with too much cash can now buy 1000GB 3.5" hard drives and 200GB 2.5" hard drives and the slightly smaller models are available at very affordable prices for the rest of us. With so much storage capacity available and so much data and information being produced everyday we should be filling our hard drives as fast as the manufacturers can build them. But aside from the torrent leeches, there seems to be a large number of computer users with a conflicting combination of ample free space and insufficient backups. When the average computer user buys a new machine with hundreds of gigabytes of storage, which never sees more than 30% utilization, and still manages to lose an important email or accidentally overwrite their holiday photo collection you have to start thinking that something isn't right.

The computer industry started it's life in a world of limited memory and limited processing power. At every opportunity low level optimizations were made to wring the most out of every instruction cycle and squeeze the most into every data bit. At the time it was necessary and it generated some great concepts and algorithms that still apply today, but it causes many problems too. The most infamous example is the Y2K bug. We squeezed years into two digits (or even less) because it was all we needed and all we could afford but as technology improved and became more accessible, the methods didn't keep pace. The same ideas behind using the least bits for date storage are still applied to storage in general.

The tasks we perform on our computers daily require the user to take responsibility for deciding what files they would like to save, which revisions of a document should be tracked, and how big their Internet files cache and Recycle Bin should be. This is an approach which is important where storage is limited. However, with gigabytes galore to spare my computer should save every file I touch, track every revision of every document, keep every web page I ever visit and shouldn't even bother me with the idea of deleting something let alone the concept of a Recycle Bin. Of course, I'm not the first person to realise this, and we are slowly beginning to see it in action today.

Vista is using the Volume Shadow Copy system to provide previous file versions to all users via easy menus in Windows Explorer and Common File Dialogs. I understand Mac OSX has been doing something very similar too. Microsoft has a new Home Server product in beta designed to completely backup all machines on a small network and allowing the user to restore to any point in time. Red-Gate SQL Compare, one of my essential tools, just saves any comparison project you create and doesn't even bother you to choose a location. I'm sure there are many other examples I can't think of right now and many more I haven't seen.

Next time you are building software think about how it is handling its data. Does it require the user to explicitly save? Does it discard data or keep it by default? If it changes data does it store the data as it was before the change? What would be involved to change the default behaviour to keep everything? In all but the most trivial cases it gets quite complicated. At the most basic level you need to decide how to store all this extra information. You then need to give the user the ability to find and retrieve what they want from the increasing hoard of information. And eventually, when you store everything and you start running out of space, you need to decide which data is no longer required and you need to decide whether to delete it, archive it to offline storage, or insist the user install another hard drive.

Ultimately many of these issues will best be solved by the underlying architecture and different approaches will need to be used for company documents versus files in the Internet cache. I imagine there is some very interesting research going into these problems and I look forward to the systems of the future where I don't have to worry about losing that family photo or overwriting my budget plan. If you know of any software that is already taking the keep everything approach, or you are building a system yourself that will do just that, leave a comment, I'd be fascinated to read about your experiences.

Now, if only I could remember where I saved that article about temporal databases...
