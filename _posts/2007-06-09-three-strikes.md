---
layout: post
permalink: http://blog.stangroome.com/?p=102
title: Three Strikes
description: None
date: 2007-06-09 01:17:25 -0000
last_modified_at: 2007-06-09 01:17:25 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I bought a LinkSys WAG54Gv2 ADSL wireless modem a couple of years ago. It had a good price and listed many features but I've suffered with unreliable WiFi since and eventually added a standalone access point to my home network.

The local broadband community has [a lot of complaints about the LinkSys modem](http://forums.whirlpool.net.au/forum-replies.cfm?t=312908) too and the original v1 [wasn't any better](http://forums.whirlpool.net.au/forum-replies.cfm?t=205861). For a company advertised as "[A Division of Cisco](http://www.google.com/search?q=linksys+division+of+cisco)", I was hoping for much more and I wasn't the only one.

As a result I usually steer friends toward something like the [D-Link DSL-G604T](http://www.dlink.com.au/Products.aspx?PID=50), a modem that I've installed many times with reliable results. Recently though, a close friend, finally moving from dial-up to broadband, purchased a LinkSys WAG54Gv3 against my advice. I guess the price, the Cisco brand association and the feature list were more convincing than my protests.

Last night I helped install the new modem and configure a Notebook, a Wii, and an iPaq (sounds like the beginning of [a joke](http://en.wikipedia.org/wiki/Meta-joke)) to connect to it wirelessly. A fourth player, a wired HP desktop PC, was used to initially configure the modem and couldn't have been easier. WiFi however, as it turned out, actually was a joke.

On all three devices, IP addresses were not being issued by the modem if WEP encryption was enabled but WAP and also no-encryption were fine. The fact that I tried WEP first is entirely another blog post but can be summarised as such: better than no encryption, combined with MAC filter, LinkSys overheat with WAP, and minimal possible damage from an attack.

DHCP over WiFi has been [a recurring issue](http://kbserver.netgear.com/kb_web_files/n101496.asp "No guarantee of DHCP passthrough") in my experience and it is very disappointing. It is incredibly fundamental and there seems to be an amazing lack of thought put into getting this functionality right. The WAG54Gv3 is new to the market and doesn't have too much feedback yet, but so far it looks like it could be LinkSys' third strike.

I've said before that [buying the more expensive product in the first place](http://www.codeassassin.com/blog/PermaLink,guid,44ad7e94-14e9-44ca-a6c5-faf2315dc394.aspx) would probably solve all this messing around, but where should the line be drawn? An entry level [Cisco 857 ADSL modem router](http://www.cisco.com/en/US/products/ps6197/index.html) will easily set you back AU$700, that's double the price of almost every other option.
