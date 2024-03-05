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

Examples of use cases that fit this description can be found [here](/use).

## Manual

Welcome to the world of this powerful command line utility. No longer will project environments clash with your host operating system, nor will you be limited to a single OS version.

Dive into this manual and unlock the potential to seamlessly switch between versions, isolate projects, and avoid software conflicts on your host system. Prepare to embark on a journey of efficient development and testing with **Toolbx** as your trusty companion.

* [Basics](https://github.com/containers/toolbox/blob/main/doc/toolbox.1.md) ­— Overview of the `toolbox(1)` command.
* [Creating Toolbx Containers](https://github.com/containers/toolbox/blob/main/doc/toolbox-create.1.md)
* [Launching / Entering Containers](https://github.com/containers/toolbox/blob/main/doc/toolbox-enter.1.md)
* [Running Commands in Containers](https://github.com/containers/toolbox/blob/main/doc/toolbox-run.1.md)
* [Listing Containers & Images](https://github.com/containers/toolbox/blob/main/doc/toolbox-list.1.md)
* [Removing Containers](https://github.com/containers/toolbox/blob/main/doc/toolbox-rm.1.md)
* [Removing Images](https://github.com/containers/toolbox/blob/main/doc/toolbox-rmi.1.md)
* [Configuration File](https://github.com/containers/toolbox/blob/main/doc/toolbox.conf.5.md) — The `toolbox.conf(5)` file format.
* [Help](https://github.com/containers/toolbox/blob/main/doc/toolbox-help.1.md) — Read the manual from the CLI.

## Custom Images

There are two different ways of creating a custom image. The first one consists on starting from scratch by creating a container file or using an existing one. In the following link, we can see an example of a [Containerfile](/example-container-file). The other possibility consists on customizing an existing image.

The following steps can be followed in order to create a custom image:

1. Create a Toolbx - in this case I'm using a custom image as base

   ```console
   $ toolbox create -i quay.io/toolbx/ubuntu-toolbox:22.04
   Created container: ubuntu-toolbox-22.04
   Enter with: toolbox enter ubuntu-toolbox-22.04
   ```

2. Enter the Toolbx and do modifications

   ```console
   $ toolbox enter ubuntu-toolbox-22.04
   ⬢$ sudo apt update && sudo apt --yes install neofetch
   Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
   Get:2 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
   Get:3 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [802 kB]
   Get:4 http://security.ubuntu.com/ubuntu jammy-security/main Translation-en [169 kB]
   Get:5 http://security.ubuntu.com/ubuntu jammy-security/main amd64 c-n-f Metadata [11.3 kB]
   Get:6 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [884 kB]
   …
   …
   …
   ```

3. Exit the Toolbx:

   ```console
   ⬢$ exit
   ```

4. commit the changes:

   ```console
   $ podman commit ubuntu-toolbox-22.04 my-custom-image
   ```

5. You can now create new custom Toolbxes by doing:

   ```console
   $ toolbox create -i my-custom-image
   ```
