---
layout: post
permalink: http://blog.stangroome.com/2017/08/16/beware-docker-and-sysctl-defaults-on-gce/
title: Beware Docker and sysctl defaults on GCE
description: None
date: 2017-08-16 11:00:40 -0000
last_modified_at: 2017-08-16 11:54:11 -0000
publish: true
pin: false
categories:
- Uncategorized
tags:
- Docker
- Google Cloud Platform
---
On Google Compute Engine (GCE) the latest VM boot images (at the time of writing) for Ubuntu 14.04 and 16.04 (eg `ubuntu-1604-xenial-v20170811`) ship with a file at [/etc/sysctl.d/99-gce.conf](https://gist.github.com/jstangroome/be4bee805c97063c28869ff4d636ed26) which contains:
  
        net.ipv4.ip_forward = 0

This kernel parameter determines whether [packets can be forwarded between network interfaces](https://www.kernel.org/doc/Documentation/networking/ip-sysctl.txt). On its own, the presence of this line isn't a big deal. Separately, when you start the Docker daemon (at least in version 17.06.0-ce), it [sets this kernel parameter to 1](https://docs.docker.com/engine/userguide/networking/default_network/container-communication/#communicating-to-the-outside-world) (assuming you haven't specified `--ip-forward=false` in the Docker configuration). Docker needs packet forwarding enabled so that Docker containers using the default bridge network can communicate outside the host. If you later execute `sysctl --system` or similar after has Docker has started, for example to apply a new value for the [nf_conntrack_max kernel parameter](https://www.kernel.org/doc/Documentation/networking/nf_conntrack-sysctl.txt) that you've specified in another file under `/etc/sysctl.d/`, then the `ip_forward` parameter will revert to 0 care of GCE's default conf file. At this point you'll find your containers cannot reach the outside world, for example this will fail to resolve:
  
        docker run ubuntu:16.04 getent hosts google.com

This will remain broken for all existing or new containers until you set the `ip_forward` parameter back to 1 manually or by restarting the Docker daemon. If you're using any Docker version since v1.8 (released about 2 years ago) you should see the following message when running a container with bridge networking if IP forwarding is disabled:
  
        WARNING: IPv4 forwarding is disabled. Networking will not work.

Of course, that only helps if you're using `docker run` interactively and does not help if the parameter gets changed after the containers are already running. If you're in this situation, add your own file to `/etc/sysctl.d/` that follows `99-gce.conf` alphabetically (eg `99-luftballon.conf`) and ensure it contains:
  
        net.ipv4.ip_forward = 0

You may also want to ensure the file has a trailing `LF` character to avoid any issues with processing it. You can check the current value of the `ip_forward` kernel parameter with one of these two commands:
  
        sysctl net.ipv4.ip_forward
    cat /proc/sys/net/ipv4/ip_forward
