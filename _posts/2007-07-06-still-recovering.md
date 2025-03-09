---
layout: post
permalink: http://blog.stangroome.com/?p=80
title: Still Recovering
description: None
date: 2007-07-06 13:07:40 -0000
last_modified_at: 2007-07-06 13:07:40 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
A new [HP Compaq 6710b business notebook](http://search.hp.com/query.html?qt=hp+compaq+6710b) arrived at work the other day. It was very shiny, had plenty of grunt, and was preloaded with Vista... apparently. Somehow, during the excitement, the notebook was turned on for the first time and then the lid was closed again before Windows had started. We didn't want to go through the initial setup just then, we had more important things to do.

Later, the time finally came to setup the new notebook and install and configure all the usual goodies. Unfortunately, just after the BIOS POST and before Vista started loading, we were greeted with an Vista boot loader error message: "_\windows\system32\winload.exe could not be loaded because the application is missing or corrupt_ ". All other options in Windows Boot Manager lead to the same result and no amount rebooting was going to help.

The answer to this problem, as described on the Boot Loader error screen, is to boot from the Windows installation disc. However HP like many OEMs today, do not ship any CDs with the hardware. To get the recovery disc you would normally burn them after setting up Windows for the first time (Catch-22) or order them from HP (takes about three business days).

Next idea: to be able burn the recovery discs after setting up Windows, HP must, like other brand notebooks, have the recovery images on the hard drive in a hidden partition. I downloaded the user guide from the HP website to find out how to use the recovery images on the hard drive. Option 1, run the HDD based recovery tool from the Start Menu (uh...) or Option 2, press F11 while booting to start the recovery tool without Windows. Excellent.

F11, F11, F11, F11,... doesn't work. POST screen lists others but not F11. Googling suggests that the HP boot-time recovery tool is not a BIOS feature but a boot loader on the recovery partition and the active partition is set wrong if F11 doesn't work. Ok, what is the easiest way to change the active partition on a non-booting PC?

We booted from a Windows XP install disc to use the Windows Recovery Console. XP install couldn't detect a hard drive in the notebook and refused to go any further. We created a bootable floppy with a USB floppy drive on an XP machine and copied diskpart.exe to it. Diskpart requires the full Win32 environment to work. We created a bootable Windows ME boot floppy but [FDISK is unusable on drives larger than 137GB](http://support.microsoft.com/kb/263044).

Lastly, we waited for the [Ultimate Boot CD](http://www.ultimatebootcd.com/) to download from a very slow mirror and we got [Ranish Partition Manager](http://www.ranish.com/part/) running on the notebook. Ranish showed the notebook's hard drive correctly as a 160GB drive. It also showed that there was a single 40GB active partition formatted as NTFS and the rest of the drive was unpartitioned. Bugger.

We are now waiting for the recovery discs to arrive in the post from HP. I'm guessing that the guys at HP who setup the initial Vista image for this notebook range forgot to leave the recovery partition intact. Just goes to show though, shut down your PC properly.
