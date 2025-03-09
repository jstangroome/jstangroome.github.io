---
layout: post
permalink: http://blog.stangroome.com/2007/06/22/drive-by-powershelling/
title: Drive By PowerShelling
description: None
date: 2007-06-22 01:54:37 -0000
last_modified_at: 2010-07-18 22:36:42 -0000
publish: true
pin: false
categories:
- PowerShell
tags: []
---
I have been using an external USB hard drive to take backup copies of certain files on several machines. Each machine has different drive configurations though so the USB drive will be assigned different drive letters on each machine. I could have found an unused drive letter common to all the machines and assign it to USB drive on the first use but that's no fun. I decided to do it the PowerShell way. I wrote a small function to find a System.IO.DriveInfo object by providing the volume label and then I can get the drive letter and pass it to Copy-Item to do the work. function Get-DriveInfo ([string] $label = $(throw "Please specify a volume label.")) { [System.IO.DriveInfo]::GetDrives() | ? { $_.IsReady -eq $True -and $_.VolumeLabel -match $label } }
