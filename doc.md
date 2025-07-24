---
layout: default
fedora-version: 39
ubuntu-version: 22.04
---

<picture class="full pixels">
    <source srcset="../assets/man-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/man.gif">
</picture>

## About

**Toolbx** is a tool for Linux, which allows the use of interactive command line environments for software development and troubleshooting the host operating system, without having to install software on the host. It is built on top of [Podman](https://podman.io/) and other standard container technologies from [OCI](https://opencontainers.org/).

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

**Toolbx** has built-in support for images corresponding to different operating system [distributions](/distros). It's also possible to create custom Toolbx images, and use them with the `image` [command line](https://github.com/containers/toolbox/blob/main/doc/toolbox-create.1.md) and [configuration file](https://github.com/containers/toolbox/blob/main/doc/toolbox.conf.5.md) options.

### From a Containerfile

One way of creating a custom Toolbx image is to define its contents in a [Containerfile](https://github.com/containers/common/blob/main/docs/Containerfile.5.md) and then use `podman build --squash` to build the image.

The easiest option is to base the custom image on one of the built-in images.

Here's a Containerfile for a custom image that adds [Emacs](https://www.gnu.org/software/emacs/), [GCC](https://gcc.gnu.org/) and [GDB](https://www.sourceware.org/gdb/) to the built-in `fedora-toolbox:{{ page.fedora-version }}` image available from `registry.fedoraproject.org`.

```conf
FROM registry.fedoraproject.org/fedora-toolbox:{{ page.fedora-version }}

ARG NAME=my-fedora-toolbox
ARG VERSION={{ page.fedora-version }}
LABEL com.github.containers.toolbox="true" \
      name="$NAME" \
      version="$VERSION" \
      usage="This image is meant to be used with the toolbox(1) command" \
      summary="Image for creating Fedora Toolbx containers"

RUN dnf --assumeyes install emacs gdb gcc
RUN dnf clean all
```

It's also possible to start from any OCI image, as long as the `com.github.containers.toolbox="true"` label is mentioned in the Containerfile.

The Containerfile can then be built to create a `my-fedora-toolbox:{{ page.fedora-version }}` image:
```console
[user@hostname ~]$ podman build \
                     --squash \
                     --tag localhost/my-fedora-toolbox:{{ page.fedora-version }} \
                     /path/to/Containerfile/dir
```

### From a Container

Another possibility is to create a custom Toolbx image from an existing Toolbx container by using `podman commit --squash`.

Here's how to create a custom image similar to the one above, but based on a Toolbx container created from the built-in `ubuntu-toolbox:{{ page.ubuntu-version }}` image available from `quay.io/toolbx`.

Create the Toolbx container:
```console
[user@hostname ~]$ toolbox create --distro ubuntu --release {{ page.ubuntu-version }}
Created container: ubuntu-toolbox-{{ page.ubuntu-version }}
Enter with: toolbox enter ubuntu-toolbox-{{ page.ubuntu-version }}
```

Alter it by installing Emacs, GCC and GDB:
```console
[user@hostname ~]$ toolbox enter ubuntu-toolbox-{{ page.ubuntu-version }}
⬢[user@toolbox ~]$ sudo apt update
Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:3 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [1208 kB]
…
…
…
⬢[user@toolbox ~]$ sudo apt --yes install emacs gcc gdb
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
…
…
…
⬢[user@toolbox ~]$ exit
```

A `my-ubuntu-toolbox:{{ page.ubuntu-version }}` image can then be created from the altered container:
```console
[user@hostname ~]$ podman commit \
                     ubuntu-toolbox-{{ page.ubuntu-version }} \
                     localhost/my-ubuntu-toolbox:{{ page.ubuntu-version }}
```
