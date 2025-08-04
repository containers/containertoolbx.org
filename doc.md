---
layout: default
fedora-version: 42
ubuntu-version: 22.04
---

<picture class="full pixels">
    <source srcset="../assets/man-dark.gif" media="(prefers-color-scheme: dark)">
    <img src="../assets/man.gif">
</picture>

## About

**Toolbx** is a tool for Linux, which allows the use of interactive command line environments for software development and troubleshooting the host operating system, without having to install software on the host. It is built on top of [Podman](https://podman.io/) and other standard container technologies from [OCI](https://opencontainers.org/).

Toolbx environments have seamless access to the user's home directory, the Wayland and X11 sockets, networking (including Avahi and CA certificates), removable devices (like USB sticks), systemd journal, SSH agent, D-Bus, ulimits, /dev and the udev database, etc.. The host file system can be accessed at `/run/host`.

This is particularly useful on [OSTree](https://ostreedev.github.io/ostree/) based operating systems like
[Fedora CoreOS](https://fedoraproject.org/coreos/) and [Silverblue](https://fedoraproject.org/silverblue/). The intention of these systems is to discourage installation of software on the host, and instead install software as (or in) containers — they mostly don't even have package managers like DNF or YUM. This makes it difficult to set up a development environment or troubleshoot the operating system in the usual way.

Toolbx solves this problem by providing a fully mutable container within which one can install their favourite development and troubleshooting tools, editors and SDKs. For example, it's possible to do `yum install ansible` without affecting the base operating system.

However, this tool doesn't *require* using an OSTree based system. It works equally well on Fedora Workstation and Server, and that's a useful way to incrementally adopt containerization.

The Toolbx environment is based on an [OCI](https://www.opencontainers.org/) image. On Fedora this is the `fedora-toolbox` image. This image is used to create a Toolbx container that offers the interactive command line environment.

Note that Toolbx makes no promise about security beyond what's already available in the usual command line environment on the host that everybody is familiar with.

Examples of use cases that fit this description can be found [here](/use).

* Table of Contents
{:toc}

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

## Container Configuration

**Toolbx** environments have seamless access to the user's home directory, the Wayland and X11 sockets, networking (including Avahi and CA certificates), removable devices (like USB sticks), systemd journal, SSH agent, D-Bus, ulimits, /dev and the udev database, etc.. The host file system can be accessed at `/run/host`.

Note that Toolbx makes no promise about security beyond what's already available in the usual command line environment on the host that everybody is familiar with.

The user ID and account details from the host operating system are propagated into the Toolbx containers, SELinux label separation is disabled, and the containers have access to the host's Kerberos credentials cache if it's configured to use KCM caches. Crucial configuration files, such as `/etc/host.conf`, `/etc/hosts`, `/etc/localtime`, `/etc/machine-id`, `/etc/resolv.conf` and `/etc/timezone`, inside the containers are kept synchronized with the host.

Toolbx containers can be identified by the `com.github.containers.toolbox` label with various Podman commands (like [inspect](https://docs.podman.io/en/latest/markdown/podman-inspect.1.html)) or by the presence of the `/run/.toolboxenv` file. The `/run/.containerenv` file contains some metadata about the containers.

The entry point of the containers is the [toolbox init-container](https://github.com/containers/toolbox/blob/main/doc/toolbox-init-container.1.md) command. It plays a role in setting up the container and mitigating the immutable nature of the configuration of [OCI](https://www.opencontainers.org/) containers.

## Custom Images

**Toolbx** has built-in support for images corresponding to different operating system [distributions](/distros). It's also possible to create custom Toolbx images, and use them with the `image` [command line](https://github.com/containers/toolbox/blob/main/doc/toolbox-create.1.md) and [configuration file](https://github.com/containers/toolbox/blob/main/doc/toolbox.conf.5.md) options.

Custom Toolbx images can be created in different ways and they need to satisfy the requirements listed below.

### From a Containerfile

One way of creating a custom Toolbx image is to define its contents in a [Containerfile](https://github.com/containers/common/blob/main/docs/Containerfile.5.md) and then use `podman build --squash` to build the image. The easiest is to base the custom image on one of the built-in images, instead of any OCI image or starting from scratch.

Here's a Containerfile for a custom image that adds [Emacs](https://www.gnu.org/software/emacs/), [GCC](https://gcc.gnu.org/) and [GDB](https://www.sourceware.org/gdb/) to the built-in `fedora-toolbox:{{ page.fedora-version }}` image available from `registry.fedoraproject.org`.

```conf
FROM registry.fedoraproject.org/fedora-toolbox:{{ page.fedora-version }}
RUN dnf --assumeyes install emacs gdb gcc
RUN dnf clean all
```

The Containerfile can then be built to create a `my-fedora-toolbox:{{ page.fedora-version }}` image:
```console
[user@hostname ~]$ podman build \
                     --squash \
                     --tag localhost/my-fedora-toolbox:{{ page.fedora-version }} \
                     /path/to/Containerfile/dir
```

### From a Container

Another possibility is to create a custom Toolbx image from an existing container by using `podman commit --squash`. The easiest is to use a Toolbx container, instead of any OCI container.

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
                     --squash \
                     ubuntu-toolbox-{{ page.ubuntu-version }} \
                     localhost/my-ubuntu-toolbox:{{ page.ubuntu-version }}
```

### Requirements

Toolbx environments are very specifically configured OCI containers. Therefore, the OCI images from which these containers are created need to satisfy some requirements.

The key words "MUST", "MUST NOT", and "SHOULD" are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

#### Commands

Images MUST have these commands:
* `capsh(1)`
* `mount(8)`
* `passwd(1)`
* `test(1)`
* `useradd(8)`
* `usermod(8)`

Images MUST have these commands to be used on host operating systems with the proprietary NVIDIA driver:
* `ldconfig(8)`

Toolbx configures containers in very specific ways. The absence of any of these tools can prevent the [toolbox enter](https://github.com/containers/toolbox/blob/main/doc/toolbox-enter.1.md) and [toolbox run](https://github.com/containers/toolbox/blob/main/doc/toolbox-run.1.md) commands from working.

Images SHOULD have these commands:
* `flatpak-spawn(1)`

Otherwise, it won't be possible to use `toolbox(1)` inside containers created from those images.

#### Entry Point

Images MUST NOT specify any entry point. This can be checked with:
```console
[user@hostname ~]$ podman inspect \
                     --format {% raw %}'{{ .Config.Entrypoint }}'{% endraw %} \
                     --type image \
                     quay.io/toolbx/arch-toolbox:latest
[]
```

Images created from base images with entry points can remove them through a Containerfile:
```conf
ENTRYPOINT []
```

Toolbx specifies the entry points of containers in a certain way. If images specify their own entry points then it will prevent the [toolbox enter](https://github.com/containers/toolbox/blob/main/doc/toolbox-enter.1.md) and [toolbox run](https://github.com/containers/toolbox/blob/main/doc/toolbox-run.1.md) commands from working.

#### Label

Images SHOULD have the `com.github.containers.toolbox="true"` label, if they fulfill these requirements.

This can be achieved through a Containerfile:
```conf
LABEL com.github.containers.toolbox="true"
```

… or `podman commit`:
```console
[user@hostname ~]$ podman commit \
                     --change 'LABEL com.github.containers.toolbox="true"' \
                     …
```

The label is meant to be used by maintainers of images to indicate that they have read this document and tested that their images work with Toolbx. Otherwise, the images won't be fully recognized by Toolbx.

#### Name

Images SHOULD be uniquely named so that they don't collide with those created by others, and their names should reflect their purpose. For example, `ubuntu-toolbox` is a better name than `toolbox` because the `ubuntu-` prefix uniquely identifies it and states its purpose.

By default, Toolbx containers are named after their corresponding images. If the image has a tag, then the tag is included in the name of the container, but it's separated by a hyphen, not a colon. For example, the default name for containers created from the `arch-toolbox:latest` images will be `arch-toolbox-latest` and those from the `fedora-toolbox:{{ page.fedora-version }}` images will be `fedora-toolbox-{{ page.fedora-version }}`.

#### Name Service Switch

Images SHOULD have the [nss-myhostname](https://www.freedesktop.org/software/systemd/man/latest/nss-myhostname.html) or `libnss_myhostname.so.2` plugin for the [Name Service Switch](https://www.gnu.org/software/libc/manual/html_node/Name-Service-Switch.html) (or NSS) functionality of the GNU C Library.

Images SHOULD prioritize this `myhostname` service sufficiently high for the `hosts` database in the NSS [configuration file](https://www.gnu.org/software/libc/manual/html_node/NSS-Configuration-File.html), [nsswitch.conf(5)](https://man7.org/linux/man-pages/man5/nsswitch.conf.5.html). Otherwise, it [can](https://github.com/authselect/authselect/pull/366) [cause](https://bugzilla.redhat.com/show_bug.cgi?id=2291062) timeouts or errors when resolving the hostnames of containers created from those images.

For example, this is a valid configuration:
```conf
hosts:  files myhostname mdns4_minimal [NOTFOUND=return] resolve [!UNAVAIL=return] dns
```

… and this is not:
```conf
hosts:  files mdns4_minimal [NOTFOUND=return] resolve [!UNAVAIL=return] myhostname dns
```

#### Paths

Toolbx expects the following paths to have the specified attributes:
* `/etc/host.conf`: If present, MUST be a regular file.
* `/etc/hosts`: If present, MUST be a regular file.
* `/etc/krb5.conf.d`: If present, MUST be a directory where the containers' `root` user can create a regular file.
* `/etc/localtime`: If present, MUST be a regular file.
* `/etc/machine-id`: If present, MUST be a regular file.
* `/etc/resolv.conf`: If present, MUST be a regular file.
* `/etc/timezone`: If present, MUST be a regular file.

#### PKCS #11

Images SHOULD use the `/etc/pkcs11/modules` directory to configure [PKCS #11](https://en.wikipedia.org/wiki/PKCS_11) modules. If the directory is present then it MUST be possible for the containers' `root` user to create a regular file.

Images SHOULD have the [p11-kit-client.so](https://p11-glue.github.io/p11-glue/p11-kit/manual/p11-kit.html) PKCS #11 module at one of these paths:
* `/usr/lib/pkcs11/p11-kit-client.so`
* `/usr/lib/aarch64-linux-gnu/pkcs11/p11-kit-client.so`
* `/usr/lib/x86_64-linux-gnu/pkcs11/p11-kit-client.so`
* `/usr/lib64/pkcs11/p11-kit-client.so`

Otherwise, certificates from certificate authorities (or CAs) that are available inside containers created from those images won't be kept synchronized with the host operating system.

File an [issue](https://github.com/containers/toolbox/issues/new) if support for a different path is needed.

#### Shells & Terminals

Images SHOULD contain the following start-up snippets for [Bash](https://www.gnu.org/software/bash/), [C shell](https://en.wikipedia.org/wiki/C_shell) and [Z shell](https://www.zsh.org/) provided by [VTE](https://gitlab.gnome.org/GNOME/vte):
* `/etc/profile.d/vte.csh`
* `/etc/profile.d/vte.sh`

Otherwise, VTE-based terminal emulators will lack some features like remembering the current working directory.

#### sudo(8)

Images MUST have `sudo(8)` enabled for users belonging to either the `sudo` or `wheel` groups, and the group itself MUST exist.

Otherwise, it will prevent the [toolbox enter](https://github.com/containers/toolbox/blob/main/doc/toolbox-enter.1.md) and [toolbox run](https://github.com/containers/toolbox/blob/main/doc/toolbox-run.1.md) commands from working. File an [issue](https://github.com/containers/toolbox/issues/new) if support for a different group is really needed. However, it's preferable to keep this list as short as possible.

Images SHOULD allow empty passwords for `sudo(8)`. This can be achieved by either adding the `nullok` option to the `PAM(8)` configuration, or by adding the `NOPASSWD` tag to the `sudoers(5)` configuration.
