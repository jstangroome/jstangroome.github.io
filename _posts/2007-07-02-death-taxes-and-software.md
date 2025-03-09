---
layout: post
permalink: http://blog.stangroome.com/?p=82
title: Death, Taxes and Software
description: None
date: 2007-07-02 13:01:06 -0000
last_modified_at: 2007-07-02 13:01:06 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
<![CDATA[

[![Tax Calculator](http://www.codeassassin.com/blog/content/binary/WindowsLiveWriter/DeathTaxesandSoftware_13268/taxcalc_1.jpg)](http://www.flickr.com/photos/phillip/345829246/) The new 2007/2008 financial year has begun and as promised the [Australian Taxation Office](http://www.ato.gov.au/) released the latest version of their [e-tax software](http://www.ato.gov.au/etax/) on July 1st. As I have for the past several years, I downloaded and installed the software and have started entering my income and deductions for the last 12 months.

When the official paperwork arrives, I'll confirm all my figures and submit my tax return electronically and receive my rebate usually within a week, deposited directly to my bank account. Considering the complexity of the income tax system, the software is excellent for non-accountant-types to complete their own tax return.

Unfortunately, e-tax 2007 is still stuck in the obsolete Windows administrator user world. Installation defaults to "c:\etax2007\". It has been over twelve years since Windows 95 was released and the standard was set for software to install into Program Files.

A single e-tax installation on a PC allows multiple people to fill in and submit their tax return, saving their work in progress locally until it is ready to send. However, the software assumes the user is an administrator and writes the *.tax progress file to the installation folder instead of Documents & Settings.

At several stages throughout the questionnaire, summaries and important details are presented to the user with a recommendation to print the information. However, the software always uses the default printer with the default settings, offering no dialogs, and only picking up changes to the default printer after restarting the software.

One of the biggest reasons why e-tax is so great is because the questions are presented in easy to understand language. Whenever more detail is required there are always hyperlinked keywords or a help button leading to a more extension definition with examples. Sadly, it's all useless under Vista because they've stayed with the [deprecated WinHelp format](http://support.microsoft.com/kb/917607) for the documentation.

Lastly, for users of Apple computers (and presumably other OSes), [the official solution](http://www.ato.gov.au/individuals/content.asp?doc=/content/73931.htm) is to use virtualization. I wonder if the purchase price of VMWare or Parallels and a Windows licence can be claimed as a cost of preparing the tax return.

By all appearances e-tax is a full Win32 application, probably written in C++ at first glance, and is expected to continue in that style for the majority of users next financial year too. Perhaps by 2009, they will have moved to a web-based client, which will open the system to non-Windows users but probably introduce other problems along the way.

]]>
