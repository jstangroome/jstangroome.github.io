---
permalink: http://blog.stangroome.com/2018/06/25/minikube-and-wsl/
title: minikube and WSL
description: None
date: 2018-06-25 12:54:14 -0000
last_modified_at: 2018-06-26 04:56:28 -0000
publish: true
pin: false
categories:
- Uncategorized
tags:
- kubernetes
- wsl
---
I develop services that run on Kubernetes. During development [minikube](https://github.com/kubernetes/minikube) provides an convenient way to run a local Kubernetes "cluster" regardless of whether you use Windows, OS X, or a Linux distribution as your host OS. Day-to-day I use minikube on Windows 10 and I prefer to use the Windows Subsystem for Linux (WSL) bash shell to have a scripting environment consistent with my colleagues, some of whom do not use Windows, and consistent with the CI system. The Linux binary of minikube isn't very useful in WSL since it doesn't support the Hyper-V driver and the Virtualbox driver cannot deal with the path differences it sees within WSL compared to those reported by `VboxManage.exe`. However, when running the Windows `minikube.exe` binary, many of the commands (e.g. `start`, `stop`, `ip`, `dashboard`) just work without any special configuration. Furthermore, creating a symlink so minikube can be executed on the PATH without the `.exe` extension easily improves the default experience. Beyond these initial commands though, some extra effort is required. SSH can be a little flakey with `minikube ssh` so I find it better to create an alias to use WSL's ssh client:
  
        ssh -a -i "$(wslpath -u "$(minikube ssh-key)")" -l docker "$(minikube ip)"

You may get an error from this SSH command that the permissions of the identity file are too open. This is fixed in two steps. Firstly ensure you have added `metadata` to the [automount options](https://blogs.msdn.microsoft.com/commandline/2018/02/07/automatically-configuring-wsl/) in your `/etc/wsl.conf` and restart your WSL session. Secondly, change WSL's view of the permissions of the key file:
  
        chmod 0600 "$(wslpath -u "$(minikube ssh-key)")"

The `minikube docker-env` command doesn't recognise the WSL environment and outputs the PowerShell environment commands instead. This can be worked around by passing the `--shell bash` arguments but the `DOCKER_CERT_PATH` environment variable value won't work with the docker Linux binary as-is and the Windows binary needs the `WSLENV` environment variable set appropriately. These extra steps are enough to justify [a helper script which I've published as a GitHub Gist](https://gist.github.com/jstangroome/f45d7d746a7f440d9684de7bb2b7c07a). With this script dot-sourced, both the Windows and Linux binaries for the Docker client will work with Minikube's Docker daemon. Lastly, use the Windows binary for `kubectl`. The paths for Kubernetes certificates in the `.kube/config` file make it difficult to use the kubectl Linux binary and so far I haven't found a problem with the Windows version.
