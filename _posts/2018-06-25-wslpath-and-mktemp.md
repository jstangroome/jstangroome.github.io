---
layout: post
permalink: http://blog.stangroome.com/2018/06/25/wslpath-and-mktemp/
title: wslpath and mktemp
description: None
date: 2018-06-25 05:17:23 -0000
last_modified_at: 2018-06-26 22:46:47 -0000
publish: true
pin: false
categories:
- Uncategorized
tags:
- wsl
---
The Windows Subsystem for Linux (aka WSL or Bash on Ubuntu on Windows) provides a fantastic reproduction of a local Linux environment without needing a virtual machine. Even better than a virtual machine, WSL includes a lot of conveniences for interoperating with the host Windows file system and processes. That is, I can access my C: drive via `/mnt/c/` and I can pop calc via `calc.exe`. Naturally, the nature of file paths in Linux and Windows are quite different so WSL performs some translations where it can (e.g. for the current working directory) and provides the `wslpath` utility for explicit conversions where necessary. Recently I discovered that even though the root filesystem of my particular WSL installation is accessible from Windows (via `%LocalAppData%/lxss/rootfs` in my case), WSL will not translate just any path within Linux to a path within this rootfs directory. And this is [because WSL is designed](https://github.com/Microsoft/WSL/issues/3146) with the idea that Windows processes should not modify WSL files. However I work with various version controlled scripts shared amongst developers on Mac, Linux, and Windows (via Cygwin mostly) that use `/tmp/` as a staging area (via `mktemp`) and when using WSL, Windows processes don't see this directory. If the current working directory is in `/tmp/`, the working directory of the Windows process will become the Windows user profile directory instead. And running `wslpath -w /tmp/` just returns `Result not representable`. To avoid modifying the shared scripts to be WSL-aware, I instead converted my WSL tmp directory to be mounted from the Windows host file system via the following set of commands. First, define the directory to use as WSL's tmp, I chose `C:\wsltemp\` out of convenience, but it could be any path you prefer.
  
        $ mkdir -p /mnt/c/wsltmp
    $ chmod 1777 /mnt/c/wsltmp # tmp dir should have the Sticky-bit set
    $ sudo chown root: /mnt/c/wsltmp
  
Also, to ensure Linux' case-sensitivity is honoured for this directory, from an elevated PowerShell, run:
  
        > fsutil.exe file setCaseSensitiveInfo c:\wsltmp enable
  
While NTFS has supported opt-in case-sensitivity for a very long time, it has only recently [supported setting it per directory](https://blogs.msdn.microsoft.com/commandline/2018/02/28/per-directory-case-sensitivity-and-wsl/). Finally, define the mount in WSL and mount it:
  
        printf '\n/mnt/c/wsltmp\t/tmp\tnone\tbind\t0\t0\n' | sudo tee -a /etc/fstab
    sudo mount -a
  
Now your WSL session, and future sessions (assuming you haven't [disabled mountFsTab](https://blogs.msdn.microsoft.com/commandline/2018/02/07/automatically-configuring-wsl/)), will have a `/tmp/` directory which will be correctly translated for Windows processes. **Warning:** if you use ssh-agent in WSL, mounting `/tmp/` to a DrvFS volume instead of LxFS will mean the ssh-agent socket (in `/tmp/ssh-*/agent.*`) will [not be available for WSL processes](https://blogs.msdn.microsoft.com/commandline/2018/02/07/windowswsl-interop-with-af_unix/) to connect, it will only be accessible by Win32 processes and therefore not useful for typical scenarios.
