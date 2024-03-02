---
layout: default
---

<picture class="full pixels">
    <source srcset="../assets/man-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/man.gif">
</picture>

## About

**Toolbx** is a tool for Linux, which allows the use of interactive command line environments for development and troubleshooting the host operating system, without having to install software on the host. It is built on top of [Podman](https://podman.io/) and other standard container technologies from [OCI](https://opencontainers.org/).

Toolbx environments have seamless access to the user's home directory, the Wayland and X11 sockets, networking (including Avahi), removable devices (like USB sticks), systemd journal, SSH agent, D-Bus, ulimits, /dev and the udev database, etc..

This is particularly useful on [OSTree](https://ostreedev.github.io/ostree/) based operating systems like
[Fedora CoreOS](https://fedoraproject.org/coreos/) and [Silverblue](https://fedoraproject.org/silverblue/). The intention of these systems is to discourage installation of software on the host, and instead install software as (or in) containers — they mostly don't even have package managers like DNF or YUM. This makes it difficult to set up a development environment or troubleshoot the operating system in the usual way.

Toolbx solves this problem by providing a fully mutable container within which one can install their favourite development and troubleshooting tools, editors and SDKs. For example, it's possible to do `yum install ansible` without affecting the base operating system.

However, this tool doesn't *require* using an OSTree based system. It works equally well on Fedora Workstation and Server, and that's a useful way to incrementally adopt containerization.

The Toolbx environment is based on an [OCI](https://www.opencontainers.org/) image. On Fedora this is the `fedora-toolbox` image. This image is used to create a Toolbx container that offers the interactive command line environment.

Note that Toolbx makes no promise about security beyond what's already available in the usual command line environment on the host that everybody is familiar with.

## Manual

Welcome to the world of this powerful command line utility. No longer will project environments clash with your host operating system, nor will you be limited to a single Arch Linux or Fedora or Ubuntu version. Dive into this manual and unlock the potential to seamlessly switch between versions, isolate projects, and avoid software conflicts on your host system. Prepare to embark on a journey of efficient development and testing with **Toolbx** as your trusty companion.

* [Basics](https://github.com/containers/toolbox/blob/main/doc/toolbox.1.md) ­— Overview of the `toolbox(1)` command.
* [Creating Toolbx Containers](https://github.com/containers/toolbox/blob/main/doc/toolbox-create.1.md)
* [Launching / Entering Containers](https://github.com/containers/toolbox/blob/main/doc/toolbox-enter.1.md)
* [Running Commands in Containers](https://github.com/containers/toolbox/blob/main/doc/toolbox-run.1.md)
* [Help](https://github.com/containers/toolbox/blob/main/doc/toolbox-help.1.md) — Getting syntax help for the CLI tools.
* [Listing Containers & Images](https://github.com/containers/toolbox/blob/main/doc/toolbox-list.1.md)
* [Removing Containers](https://github.com/containers/toolbox/blob/main/doc/toolbox-rm.1.md)
* [Removing Images](https://github.com/containers/toolbox/blob/main/doc/toolbox-rmi.1.md)
* [Toolbx Configuration](https://github.com/containers/toolbox/blob/main/doc/toolbox.conf.5.md) — Learn about the toolbx config file format.
