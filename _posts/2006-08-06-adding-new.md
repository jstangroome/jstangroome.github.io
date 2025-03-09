---
layout: post
permalink: http://blog.stangroome.com/?p=125
title: Adding New
description: None
date: 2006-08-06 03:02:35 -0000
last_modified_at: 2006-08-06 03:02:35 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

I've spent too much time in Reflector again these past weeks. One of the instigators was the BindingSource AddingNew event. If you happen to handle this event and assign your own object to e.NewObject property, don't expect the Position on the BindingSource to refer to the new item as it normally would.

 

The BindingSource explicitly checks to see if you have set e.NewObject yourself and if you have, it doesn't bother to update the Position to refer to your new item at the end of the list. You will need to set the Position property yourself too.

]]>
