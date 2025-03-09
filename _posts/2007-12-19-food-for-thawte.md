---
layout: post
permalink: http://blog.stangroome.com/?p=36
title: Food for Thawte
description: None
date: 2007-12-19 12:15:48 -0000
last_modified_at: 2007-12-19 12:15:48 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
I was trying to renew my [Thawte Personal Email Certificates](http://www.thawte.com/secure-email/personal-email-certificates/) this week because they expire very soon. Unfortunately I was having zero luck requesting a new certificate via Thawte's certificate management website.

Whenever I reached the stage to choose a Cryptographic Service Provider, the VBScript that is supposed to fill the drop down list would fail with a generic "424 Object required" error and the process would go no further. I tried several PCs but all my machines at home and work are Vista with IE7 and Thawte's list of supported software does not list Vista or IE7.

I downloaded the [Internet Explorer 6 Application Compatibility VPC image from Microsoft](http://www.microsoft.com/downloads/details.aspx?FamilyId=21EABB90-958F-4B64-B5F1-73D0A413C8EF&displaylang=en) and was able to complete the certificate renewal then export the private key to a file and move it out of the virtual machine and install it on my workstation.

I tried IE7 on Vista 32-bit, 64-bit, non-admin, Administrator, Protected Mode On and Off, and tried adding the Thawte website to my trusted sites. None of this helped. I eventually decided to download the IE7 version of the App Compat VPC to see if Vista or IE7 is the problem.

I found that apart from a security warning, which helped me to track down the ultimate reason, the Thawte website works fine with IE7 on Windows XP. The reason is that the [XEnroll.dll certificate enrolment control was replaced in Vista for a more secure CertEnroll.dll](http://technet2.microsoft.com/WindowsVista/en/library/73bdca07-a9f0-40d7-a26e-6f4f11759e4c1033.mspx?mfr=true).

As usual, I'm disgusted that it has been eleven months since Vista went RTM and Thawte still haven't addressed this issue. It's even worse because the Betas and Release Candidates of Vista were around for even longer and these distributions exist so third party software and service providers can test their systems to be ready soon after launch.
