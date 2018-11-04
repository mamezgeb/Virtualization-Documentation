---
title: Linux Containers on Windows
description: Learn about Windows containers.
keywords: docker, containers
author: scooley
ms.date: 11/02/2018
ms.topic: article
ms.prod: windows-containers
ms.service: windows-containers
ms.assetid: edfd11c8-ee99-42d8-9878-efc126fe1826
---

# Linux Containers on Windows

Right now there are two ways to run Linux containers on a Windows container host:

1. (traditional) Run Linux containers in a full Linux VM.  This is the way Docker has traditionally run Linux containers on Windows machines.
1. (new) Run Linux containers directly on Windows with [Hyper-V Isolation](../manage-containers/hyperv-container.md), referred to as LCOW (Linux Containers on Windows).

This article will explanation in detail how these approaches differ.

## Linux Containers in a Moby VM

The way 

## Linux Containers with Hyper-V isolation

Linux kernel with just enough OS to support containers.  The changes to Windows and Hyper-V to build this started in the _Windows 10 Fall Creators Update_ and _Windows Server, version 1709_, but bringing this together also required work with the open source [Moby project](https://www.github.com/moby/moby) on which Docker technology is built, as well as the Linux kernel.

Other projects are beginning to build similar highly-tuned Linux kernels for projects like Kata.

## Run a Hyper-V isolated Linux Container

Follow these [instructions](https://blog.docker.com/2018/02/docker-for-windows-18-02-with-windows-10-fall-creators-update/) from Docker.

## Work in progress

Ongoing progress in the Moby project can be tracked on [GitHub](https://github.com/moby/moby/issues/33850)

## Versioning

## Current development

### Bind mounts

Bind mounting volumes with `docker run -v ...` stores the files on the Windows NTFS filesystem, so some translation is needed for POSIX operations. Some filesystem operations are currently partially or not implemented, which may cause incompatibilities for some apps.

These operations are not currently working for bind-mounted volumes:

- MkNod
- XAttrWalk
- XAttrCreate
- Lock
- Getlock
- Auth
- Flush
- INotify

There are also a few that are not fully implemented:

- GetAttr – The Nlink count is always reported as 2
- Open – Only ReadWrite, WriteOnly, and ReadOnly flags are implemented

### Known app issues

These applications all require volume mapping, which has some limitations covered under [Bind mounts](#Bind-mounts). They will not start or run correctly.

- MySQL
- PostgreSQL
- WordPress
- Jenkins
- MariaDB
- RabbitMQ

## Extra information

[Linux Container Video](https://sec.ch9.ms/ch9/1e5a/08ff93f2-987e-4f8d-8036-2570dcac1e5a/LinuxContainer.mp4)

[LinuxKit LCOW-kernel plus build instructions](https://github.com/linuxkit/lcow)