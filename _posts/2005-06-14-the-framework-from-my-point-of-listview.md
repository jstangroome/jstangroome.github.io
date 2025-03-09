---
layout: post
permalink: http://blog.stangroome.com/?p=145
title: The framework from my point of ListView
description: None
date: 2005-06-13 15:39:40 -0000
last_modified_at: 2005-06-13 15:39:40 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

In my ongoing war with the Framework, a memorable battle worth documenting is that of the ListView and the BackgroundImage. My current major project consists of several forms that all have a large ListView control in Details view as the prime user interface element. These forms are so similar that I was able to inherit them all from a custom base Form with most of the functionality. Unfortunately, some users don't really pay attention to what is in front of them, so I decided to put a nice big picture as the background of the ListView to make things obvious.

The ListView control has a public BackgroundImage property but it has attributes preventing it from displaying in the Properties window. I tried setting BackgroundImage property via code and discovered that it has no effect. After a great deal of digging with Lutz Roeder's trusty Reflector I discovered that the problem is a combination of the ListView's ControlStyles and the Control's WmEraseBkgnd. To solve this I created a new class to inherit from ListView and catch the WM_ERASEBKGND message in an overridden WndProc. In the WndProc, I change the UserPaint and AllPaintingInWmPaint styles appropriately, pass through to MyBase.WndProc to draw the background image, and finally revert the styles to their previous values. I also overrode the BackgroundImage property's attributes to show it in the Properties window again.

This time the background image shows in the ListView but as soon as an item is added to the list it obscures the image. Reflector showed that ListViewItems have their own BackColor which defaults to the parent ListView's BackColor if not specified. I changed my code to set each ListViewItem's BackColor to Color.Transparent but it didn't help. Further digging in Reflector shows than the framework's ColorTranslator.ToWin32 method discards transparency information. To solve this I catch WM_REFLECTNOTIFY (a special modification of WM_NOTIFY) in my subclass, call MyBase.WndProc to do most of the work, obtain a NMLVCUSTOMDRAW structure from the message's LParam, then use my own transparency-aware ToWin32 method to set the item's background colour correctly.

So now the background image shows and the items don't obscure it but as the selection moves from item to item, the unselected items still appear highlighted. My code never puts more than 50 items in the ListView so I decided to take the easy way out and call Me.Invalidate in my subclass' overridden OnSelectedIndexChanged method. I should find the rectangles for the unselected item(s) and only invalidate those but I will do that later when the effort is justified.

I hoped my custom ListView would be perfect now, and it was for a while, but as soon as it contained enough items to require scrolling I noticed the background image became garbled after each scroll. The ListView does not provide OnScroll methods so I needed to catch the WM_VSCROLL and WM_HSCROLL messages and once again call Me.Invalidate. The background image is always drawn correctly now but the list flickers when scrolling and I can't justify the effort to solve that minor glitch yet.

I decided to setup a PaintBackground event (normally missing from the ListView control) so I can draw dynamic information onto the ListView at runtime and everything is just dandy now. With this much work involved I though it was worth checking if these problems are fixed in .NET 2.0. I downloaded the beta and was pleased to find many new features in the ListView control, including support for the BackgroundImage property. Sadly, testing this feature showed that the people at Microsoft also had problems with the selection highlight not clearing. I lodged a bug report and apparently it has been fixed in time for the official release. I'm glad I don't need to fix the new one myself too; .NET 2.0 is much more complicated under the hood.  

]]>
