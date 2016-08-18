---
layout: post
title: Fix Permission Denied when access mounted volume inside container
date: 2016-08-18
category: blog
tags: Fedora Docker
---

I'm using Fedora 23 and also usually use docker to run a Fedora container so
that I can have a ready-to-use environment quickly to run something, for example
verifying a RPM by installing it simply. Volume is a much useful for sharing
data between host and container, but without some magic, mounted volume inside
container is not accessible, that is Permission Denied even if just ``ls``.

This is related to the SELinux actually. I'm not a SELinux expert. I can't give
any deeper explanation why this happens. All what I knew and learned is from
this great article [Using Volumes with Docker can Cause Problems with SELinux](http://www.projectatomic.io/blog/2015/06/using-volumes-with-docker-can-cause-problems-with-selinux/).
The solution is to add suffix either ``z`` or ``Z``. For example,

```
docker run --rm -i -t -v /path/to/host/dir:/path/to/container/dir:Z fedora:24 /bin/bash
```

[Volume Labels](https://docs.docker.com/engine/tutorials/dockervolumes/#/volume-labels)
actually already describes the functionality of Z. But, it
mentions nothing that is related to this issue and SELinux. I suspect, in
Docker's documentation, Z is described in general for kinds of Linux
distributions including those that don't use SELinux at all. Anyway, hopefully,
this is useful for you to solve your problem. Happy hacking! :)
