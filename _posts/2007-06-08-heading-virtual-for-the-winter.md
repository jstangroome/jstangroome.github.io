---
layout: post
permalink: http://blog.stangroome.com/?p=105
title: Heading Virtual For The Winter
description: None
date: 2007-06-07 14:36:04 -0000
last_modified_at: 2007-06-07 14:36:04 -0000
publish: false
pin: false
categories:
- Uncategorized
tags: []
---
Last time I looked into the options for migrating physical machines to Microsoft Virtual PC/Server I only found the [Virtual Server Migration Toolkit](http://www.microsoft.com/technet/virtualserver/downloads/vsmt.mspx). I downloaded all the appropriate components and began following the installation instructions. I quickly found myself overwhelmed by a solution too complex for moving one or two desktops to virtual machines.

With a notebook refresh on the horizon I decided I should look into my options again, hoping to virtualize my old system as a form of runnable backup. I discovered an excellent collection of blog posts from over a year ago [conveniently listed here](http://blogs.virtualserver.tv/blogs/virtualmachine/default.aspx). The articles are spread out but do an excellent job to explain the common problems and how to solve them. I am summarising the process here mainly for my convenience but I'm sure it will be useful to others.

* Install the Standard/Generic IDE Controller driver on the physical machine.
* Copy an image of the physical drive into a VHD*.
* Use Safe Mode/Recovery Console to disable drivers that halt the boot.
* Replace System32\HAL.DLL with HALACPI.DLL from the Windows CD.
* Install VM Additions, uninstall unnecessary drivers.

For full details on achieving these steps, refer back to the original articles and you should end with a functional Virtual PC of your physical machine. Even better, your physical machine is untouched and still usable.

* The original articles suggest using Virtual PC's linked physical disk feature to mount the actual hard drive and then convert it to a VHD. Unfortunately, Virtual PC 2007 does not include this feature anymore, however I found this [MakeVM](http://www.sysdevsoftware.com/soft/makevm.php) software which sounds promising.

Of course, there is also the [VMWare](http://www.vmware.com/) range of virtualization products which apparently have much better [P2V](http://www.vmware.com/products/converter/) solutions.
