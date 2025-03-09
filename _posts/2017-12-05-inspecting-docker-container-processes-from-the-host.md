---
layout: post
permalink: http://blog.stangroome.com/2017/12/05/inspecting-docker-container-processes-from-the-host/
title: Inspecting Docker container processes from the host
description: None
date: 2017-12-05 08:19:54 -0000
last_modified_at: 2017-12-05 08:19:54 -0000
publish: true
pin: false
categories:
- Uncategorized
tags:
- Docker
---
While I favour a containerize-all-the-things approach to new projects I still need to maintain systems that were designed several years ago around a combination of containers and host-based applications working together. In these situations it is common enough to execute `ps` or `iotop` on the host and see all the host and container processes together with no obvious indication of which processes belong to which containers. Here I will share some simple commands to help map the host-view of a containerised process to its container. First, given a host PID, how do I know which container it belongs to?
  
        $ cat "/proc/${host_pid}/cgroup"
    ...
    4:cpu:/docker/769739f359ec192edf6c565f7756bb5ecabcfac3e691c2444794ab6a7d398e39
    ...

The [procfs](https://www.kernel.org/doc/Documentation/filesystems/proc.txt) `cgroup` file will show the full Docker container ID which you can then use with `docker inspect` to get more container details. Vice-versa if you have the container ID and want to locate the host process(es) you can use:
  
        $ sudo ps -e -o pid,comm,cgroup | grep "/docker/${cid}"

Lastly, if you're trying to debug a container process from the host and need the host-path to the process' binary I have found a method to has been working reliably. Unfortunately, because the procfs `exe` file is a symbolic link, not a hard link it won't resolve to the file within the container's layered file system so a few extra steps are required. First, read the symlink to get the fully-qualified container-path to the binary:
  
        $ exe=$(readlink "/proc/${host_pid}/exe")

Next, parse the process' memory-mapped files to locate the first memory region referencing this file path:
  
        $ map=$(grep -m1 -F "${exe}" "/proc/${host_pid}/maps" | cut -d' ' -f1)

Lastly read the symlink for this memory map from procfs' `map_files` directory:
  
        $ readlink "/proc/${host_pid}/map_files/${map}"

This final output should look something like this:
  
        /var/lib/docker/aufs/diff/3cc533dae9a6cc96d6092844be3ce78c737db793cf1493b9f47e652e96bfd71e/bin/sleep

Note that the long identifier in that path is not the container ID, nor is it available via `docker inspect`, although I'm sure someone else has posted online how to locate this path via other means.
